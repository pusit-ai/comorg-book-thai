chapter 5.3 DDR DRAM
🔹 5.3 DDR DRAM
🧠 ปัญหา
DRAM เป็น bottleneck ของระบบ
🔸 SDRAM (Synchronous DRAM)
ทำงานตาม clock
ไม่ต้องรอ (wait state)
รองรับ burst mode
🔸 DDR DRAM

เพิ่มความเร็วโดย:
1.ส่งข้อมูลทั้งขาขึ้น + ขาลงของ clock
2.เพิ่มความถี่ clock
3.ใช้ buffer (prefetch)

🔸 ตาราง DDR	
| รุ่น | Prefetch (bit) | แรงดัน (V) | Data rate (Mbps) |
| ---- | -------------- | ---------- | ---------------- |
| DDR1 | 2              | 2.5        | 200–400          |
| DDR2 | 4              | 1.8        | 400–1066         |
| DDR3 | 8              | 1.5        | 800–2133         |
| DDR4 | 8              | 1.2        | 2133–4266        |

🔸 จุดเด่น
เร็วขึ้นหลายเท่า
ใช้ bank group เพิ่ม parallelism