# 📚 บทที่ 8: Virtual Memory (หน่วยความจำเสมือน)

> **สรุปเนื้อหา**: ระบบ Virtual Memory เพื่อขยาย Main Memory ด้วย Disk และจัดการ Memory อย่างมีประสิทธิภาพ

## 📋 สารบัญหัวข้อ

| หัวข้อ | ผู้รับผิดชอบ | ลิงก์ไฟล์ |
|--------|--------------|-----------|
| **8.1** Virtual Memory Overview | นาย สิรภัทร อิ่มไพศาลสมบัติ | [👉 อ่าน 8.1](8-1-Virtual-Memory-Overview.md) |
| **8.2** Hardware and Control Structures | นาย สิรภัทร อิ่มไพศาลสมบัติ | [👉 อ่าน 8.2](8-2-Virtual-to-Physical-Address-Translation.md) |
| **8.3** Operating System Software | นาย สิรภัทร อิ่มไพศาลสมบัติ | [👉 อ่าน 8.3](8-3-Demand-Paging.md) |
| **8.4** Page Replacement Algorithms | นาย สิรภัทร อิ่มไพศาลสมบัติ | [👉 อ่าน 8.4](8-4-Page-Replacement.md) |
| **8.5** Performance of Demand Paging | นาย สิรภัทร อิ่มไพศาลสมบัติ | [👉 อ่าน 8.5](8-5-Page-Replacement-Algorithms.md) |
| **8.6** Design Issues | นาย สิรภัทร อิ่มไพศาลสมบัติ | [👉 อ่าน 8.6](8-6-Performance-of-Demand-Paging.md) |

## 🎯 หลักการสำคัญ

```
Virtual Address Space >> Physical Main Memory
Disk = Backing Store
Page Fault → Page Replacement → Performance Trade-off