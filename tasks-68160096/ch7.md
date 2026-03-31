# 📘 Chapter 7: Input / Output (I/O)

---

## 🟢 ภาพรวม
- I/O คือการสื่อสารระหว่าง **CPU / Memory กับอุปกรณ์ภายนอก**
- ตัวอย่าง: Keyboard, Mouse, Disk, Printer
- ปัญหาหลัก: ความเร็วของอุปกรณ์ไม่เท่ากัน

---

## 🔹 7.1 External Devices

### ประเภทอุปกรณ์
- **Human-readable**
  - เช่น Keyboard, Monitor
- **Machine-readable**
  - เช่น Disk, Sensor
- **Communication devices**
  - เช่น Network card, Modem

### แนวคิดสำคัญ
- อุปกรณ์แต่ละชนิดมีความเร็วต่างกัน
- ต้องมีตัวกลางช่วยจัดการ (I/O Module)

---

## 🔹 7.2 I/O Modules

### หน้าที่
- เชื่อม CPU ↔ Device
- ควบคุมการรับส่งข้อมูล

### สิ่งที่ทำ
- Control & Timing
- Data Buffering
- Error Detection

### โหมดการทำงาน
- Programmed I/O
- Interrupt-driven I/O
- DMA

---

## 🔹 7.3 Programmed I/O

### วิธีทำงาน
1. CPU สั่งอุปกรณ์
2. CPU รอ (Busy Waiting)
3. รับ/ส่งข้อมูล

### ข้อดี
- เข้าใจง่าย

### ข้อเสีย
- CPU เสียเวลา (ต้องรอ)

---

## 🔹 7.4 Interrupt-Driven I/O

### วิธีทำงาน
1. CPU สั่งงานแล้วไปทำอย่างอื่น
2. อุปกรณ์เสร็จ → ส่ง Interrupt
3. CPU กลับมาทำงาน I/O

### ข้อดี
- CPU ไม่ต้องรอ

### ข้อเสีย
- มี overhead จาก interrupt

---

## 🔹 7.5 Direct Memory Access (DMA)

### วิธีทำงาน
- อุปกรณ์ส่งข้อมูลเข้า Memory โดยตรง
- ไม่ต้องผ่าน CPU ตลอดเวลา

### ข้อดี
- เร็วมาก
- ลดภาระ CPU

### องค์ประกอบ
- DMA Controller

---

## 🔹 7.6 Direct Cache Access

### แนวคิด
- ส่งข้อมูลเข้า Cache แทน Memory

### จุดเด่น
- เร็วกว่า DMA ปกติ
- ลด Latency

---

## 🔹 7.7 I/O Channels and Processors

### แนวคิด
- ใช้ Processor แยกมาจัดการ I/O

### ข้อดี
- CPU หลักทำงานอื่นได้เต็มที่

### ประเภท
- Channel
- I/O Processor

---

## 🔹 7.8 External Interconnection Standards

### คืออะไร
- มาตรฐานการเชื่อมต่ออุปกรณ์

### ตัวอย่าง
- USB
- PCIe
- SATA

### จุดสำคัญ
- กำหนดความเร็วและรูปแบบการสื่อสาร

---

## 🔹 7.9 ตัวอย่างโครงสร้าง I/O (IBM System)

### โครงสร้าง
- CPU
- I/O Processor
- Channel
- Device

### จุดเด่น
- รองรับหลายอุปกรณ์
- ทำงานพร้อมกันได้

---

## 🔹 7.10 Key Terms

- I/O Module
- Interrupt
- DMA
- Buffer
- Bus
- Device Controller

---

## 🧠 สรุปภาพรวม

### เปรียบเทียบ 3 วิธีหลัก

| วิธี | CPU ทำงาน | ความเร็ว | ข้อเสีย |
|------|----------|---------|--------|
| Programmed I/O | ต้องรอ | ช้า | เสียเวลา |
| Interrupt I/O | ไม่ต้องรอ | ปานกลาง | มี overhead |
| DMA | ใช้น้อยมาก | เร็วมาก | ซับซ้อน |

---

## 🎯 จำให้ขึ้นใจ
- I/O = จุดช้าที่สุดของระบบ
- ต้องใช้ Interrupt หรือ DMA ช่วย
- ลดการใช้ CPU = ระบบเร็วขึ้น
