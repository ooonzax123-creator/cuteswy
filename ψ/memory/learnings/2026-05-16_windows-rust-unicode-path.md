# Windows Rust Build: Unicode Path + MSVC Gotchas

> Discovered: 2026-05-16 during AdobePremiereProMCP installation

## The Problem

1. **Unicode ψ in path**: Windows MSVC linker (`link.exe`) ไม่รองรับ Unicode characters ใน path
   - Error: `link: extra operand 'C:\...\ψ\...'`
   - Fix: Copy source ไปที่ ASCII-only path ก่อน build

2. **MSVC linker ไม่มี**: Rust default target บน Windows คือ `x86_64-pc-windows-msvc` แต่ถ้าไม่ติดตั้ง Visual Studio Build Tools จะหา `link.exe` ไม่เจอ
   - Fix: ใช้ MSYS2 + GNU target แทน

## The Solution (Windows without VS Build Tools)

```bash
# 1. ติดตั้ง MSYS2
winget install MSYS2.MSYS2

# 2. ติดตั้ง gcc + protobuf
/c/msys64/usr/bin/pacman.exe -S --noconfirm mingw-w64-x86_64-gcc mingw-w64-x86_64-protobuf

# 3. ติดตั้ง GNU Rust target
rustup toolchain install stable-x86_64-pc-windows-gnu

# 4. Configure project .cargo/config.toml
cat > .cargo/config.toml << EOF
[target.x86_64-pc-windows-gnu]
linker = "C:/msys64/mingw64/bin/gcc.exe"
ar = "C:/msys64/mingw64/bin/ar.exe"
EOF

# 5. Build
export PROTOC="/c/msys64/mingw64/bin/protoc.exe"
export PATH="$PATH:/c/msys64/mingw64/bin"
cargo +stable-x86_64-pc-windows-gnu build --release --target x86_64-pc-windows-gnu
```

## Checklist ก่อน build native Rust บน Windows

- [ ] Path มี Unicode? → Copy ไป ASCII path ก่อน
- [ ] MSVC installed? (`link.exe` อยู่ใน PATH?) → ถ้าไม่มี → MSYS2 path
- [ ] protoc ต้องการ? → `PROTOC=/c/msys64/mingw64/bin/protoc.exe`
- [ ] Path config.toml ใช้ forward slash? → ใช้ `C:/msys64/...` ไม่ใช่ `C:\msys64\...`

## Applied to

- AdobePremiereProMCP rust-engine (premierpro-media-engine.exe)
- Built at: `C:\Users\trex3\AdobePremiereProMCP\rust-engine\target\x86_64-pc-windows-gnu\release\`
