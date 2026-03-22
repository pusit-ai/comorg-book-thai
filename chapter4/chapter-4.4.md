# 4.4 Pentium 4 Cache Organization

## ภาพรวม
หัวข้อนี้อธิบายวิวัฒนาการของ cache ในตระกูล Intel และแสดงให้เห็นว่า Pentium 4 ใช้โครงสร้าง cache หลายระดับเพื่อรองรับ CPU ที่เร็วขึ้นมาก

## วิวัฒนาการสำคัญของ Intel Cache
ลำดับแนวคิดที่พัฒนามาโดยสรุปคือ

- **80386**: ยังไม่มี on-chip cache
- **80486**: เพิ่ม on-chip cache เพื่อแก้ปัญหา external memory ช้ากว่า bus
- **Pentium**: แยก L1 instruction cache และ L1 data cache เพื่อลด contention
- **Pentium Pro / Pentium II**: เพิ่ม L2 และพัฒนาเส้นทางเชื่อมต่อให้เร็วขึ้น
- **Pentium III / Pentium 4**: เพิ่ม L3 เพื่อรองรับงานที่ต้องเข้าถึงข้อมูลจำนวนมาก

## โครงสร้าง Cache ของ Pentium 4
Pentium 4 มี cache หลัก 3 ระดับที่กล่าวถึงในหัวข้อนี้

### L1 Data Cache
- ขนาด **16 kB**
- line size **64 bytes**
- แบบ **4-way set associative**

### L1 Instruction Cache
- ไม่ได้เก็บ instruction แบบเดิมโดยตรง
- แต่เก็บ **micro-operations (mops)** ที่ผ่านการ decode แล้ว
- ขนาดประมาณ **12K mops**

นี่เป็นจุดเด่นสำคัญ เพราะ instruction cache ของ Pentium 4 อยู่ **หลังขั้น decode** ไม่เหมือน Pentium รุ่นก่อนและไม่เหมือนหลายสถาปัตยกรรมทั่วไป

### L2 Cache
- ขนาด **512 kB**
- line size **128 bytes**
- แบบ **8-way set associative**
- ทำหน้าที่ป้อนข้อมูลให้กับ L1 caches

### L3 Cache
- เพิ่มใน Pentium 4 ระดับสูง
- ใช้รองรับงานที่ต้องเข้าถึงข้อมูลจำนวนมาก

## องค์ประกอบหลักของ Pentium 4 Core
จากภาพรวมของระบบ มี 4 ส่วนหลัก

### 1. Fetch / Decode Unit
- ดึงคำสั่งจาก L2 cache
- แปลงคำสั่งเป็น micro-operations
- เก็บผลลัพธ์ไว้ใน L1 instruction cache

### 2. Out-of-Order Execution Logic
- จัดตารางการประมวลผล micro-operations ใหม่ตามความพร้อมของข้อมูลและทรัพยากร
- อาจรันคำสั่งไม่ตรงตามลำดับเดิม
- รองรับ speculative execution

### 3. Execution Units
- ประมวลผล micro-operations
- ดึงข้อมูลจาก L1 data cache
- เก็บผลชั่วคราวไว้ใน register

### 4. Memory Subsystem
- รวม L2, L3 และ system bus
- ใช้เข้าถึง main memory และ I/O เมื่อ cache miss

## เหตุผลที่ Pentium 4 ใช้ Trace/Decoded Instruction Cache
แนวคิดสำคัญคือ Pentium 4 ไม่อยาก decode instruction เดิมซ้ำ ๆ ทุกครั้ง

ถ้าเก็บผลหลัง decode ไว้เลย
- ลดภาระของ decode stage
- ป้อน micro-operations ให้ execution core ได้เร็วขึ้น
- เหมาะกับสถาปัตยกรรมที่ pipeline ลึกและต้องการ throughput สูง

## ใจความสำคัญของหัวข้อนี้
- Pentium 4 แสดงให้เห็นการพัฒนา cache จาก single cache ไปสู่ multi-level cache
- L1 ถูกแยก instruction/data อย่างชัดเจน
- L1 instruction cache ของ Pentium 4 เด่นตรงที่เก็บ **decoded micro-operations**
- L2 และ L3 ถูกเพิ่มเพื่อรองรับการเข้าถึงข้อมูลจำนวนมากและลดคอขวดจาก main memory
