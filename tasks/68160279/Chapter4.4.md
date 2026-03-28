# 4.4 Replacement Algorithms and Write Policy

## Replacement Algorithms
เมื่อ Cache เต็มและต้องการนำข้อมูลใหม่เข้า ต้องมีอัลกอริทึมเลือกข้อมูลเดิมออก (สำหรับ Associative และ Set-Associative):
* **Least Recently Used (LRU):** เลือก Block ที่ไม่ได้ถูกอ้างถึงนานที่สุด (นิยมที่สุด)
* **First-In-First-Out (FIFO):** เลือก Block ที่อยู่ใน Cache มานานที่สุด
* **Least Frequently Used (LFU):** เลือก Block ที่ถูกเรียกใช้จำนวนครั้งน้อยที่สุด
* **Random:** เลือกแบบสุ่ม

## Write Policy
เมื่อข้อมูลใน Cache ถูกเปลี่ยนแปลง ต้องมีวิธีจัดการกับ Main Memory:
1. **Write Through:** เขียนข้อมูลลงทั้ง Cache และ Main Memory พร้อมกันเสมอ ข้อดีคือข้อมูลตรงกันตลอดเวลา แต่ข้อเสียคือสร้าง Traffic บน Bus สูง
2. **Write Back:** เขียนข้อมูลเฉพาะใน Cache ก่อน และจะเขียนลง Main Memory เมื่อ Block นั้นถูกเปลี่ยนออก (โดยใช้ Dirty Bit หรือ Use Bit เพื่อบอกว่ามีการแก้ไข)

## Cache Coherency
ในระบบที่มีหลาย Processor หรือมี I/O Module เข้าถึง Memory ได้ ต้องระวังปัญหาข้อมูลไม่ตรงกัน (Inconsistency) วิธีแก้เช่น:
* Bus Watching (Snooping)
* Hardware Transparency
* Noncacheable Memory