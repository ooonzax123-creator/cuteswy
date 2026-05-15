# Oracle Manager Workflow: Awaken + Install MCP

> Learned: 2026-05-16

## Pattern

เมื่อ awaken Oracle ใหม่ที่มี specialty เฉพาะ (เช่น Post-Production):
1. Awaken ก่อน (สร้าง identity, soul, philosophy)
2. Learn repos ที่เกี่ยวข้องกับ specialty นั้น
3. Install tools/MCP ที่ specialty ต้องใช้
4. บันทึก learning ไว้ใน Oracle ใหม่

## Time Estimate

- Oracle birth: ~25 min
- Complex MCP install (multi-language): ~35 min
- Total: ~60 min สำหรับ Oracle ใหม่พร้อม tools

## Windows MCP Build Checklist

ก่อน build native code บน Windows:
- [ ] Path ASCII only? (ψ = ปัญหา)
- [ ] MSVC หรือ MSYS2? (ถ้าไม่มี VS → MSYS2)
- [ ] protoc ต้องการ? (MSYS2: `pacman -S mingw-w64-x86_64-protobuf`)
