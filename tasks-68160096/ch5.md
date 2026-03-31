# Chapter 5: Internal Memory

สรุปเนื้อหาโครงสร้างและเทคโนโลยีของหน่วยความจำภายในคอมพิวเตอร์
## 📑 Table of Contents
* [5.1 Semiconductor Main Memory](#section51)
* [5.2 Error Correction](#section52)
* [5.3 DDR DRAM](#section53)
* [5.4 Flash Memory](#section54)
* [5.5 Newer Nonvolatile Solid-State Memory Technologies](#section55)
* [5.6 Key Terms, Review Questions, and Problems](#section56)
---
<a name="section51"></a>
## 5.1 Semiconductor Main Memory (หน่วยความจำหลักแบบเซมิคอนดักเตอร์)
### องค์ประกอบพื้นฐาน (Organization)
* **Memory Cell:** เป็นหน่วยที่เล็กที่สุด มีคุณสมบัติเด่นคือ มี 2 สถานะที่เสถียร (แทน 0 และ 1), สามารถเขียนข้อมูลลงไปได้ และสามารถอ่านสถานะออกมาได้ 
* **การทำงาน:** ใช้สัญญาณ **Select** เพื่อเลือกเซลล์ และสัญญาณ **Control** เพื่อกำหนดว่าจะอ่านหรือเขียน

### DRAM และ SRAM
* **DRAM (Dynamic RAM):** เก็บข้อมูลในรูปของประจุไฟฟ้าในตัวเก็บประจุ (Capacitors) มีการรั่วไหลของประจุจึงต้องมี **Refresh** ข้อมูลเป็นระยะ 
* ]**SRAM (Static RAM):** ใช้ลอจิกเกตแบบ Flip-flop เพื่อรักษาข้อมูล ไม่ต้องรีเฟรชตราบเท่าที่มีไฟเลี้ยง มีความเร็วสูงกว่า DRAM

### ประเภทของ ROM (Read-Only Memory)
เป็นหน่วยความจำประเภท **Nonvolatile** (ข้อมูลไม่หายเมื่อไฟดับ)
* **ROM:** เขียนข้อมูลถาวรตั้งแต่ตอนผลิต 
* **PROM:** เขียนได้ครั้งเดียวด้วยอุปกรณ์พิเศษหลังการผลิต 
* **EPROM:** ลบข้อมูลได้ด้วยแสง UV และเขียนใหม่ได้หลายครั้ง 
* **EEPROM:** ลบและเขียนใหม่ได้ด้วยไฟฟ้าในระดับ Byte 
<a name="section52"></a>
## 5.2 Error Correction (การแก้ไขข้อผิดพลาด)
* **ประเภทความเสียหาย:** แบ่งเป็น **Hard Failure** (ความเสียหายถาวรที่ตัวฮาร์ดแวร์) และ **Soft Error** (ความผิดพลาดชั่วคราวจากสัญญาณรบกวน)
* **Hamming Code:** กลไกการใช้ Check bits เพื่อตรวจจับและแก้ไขข้อผิดพลาด
    * **SEC (Single-Error-Correcting):** แก้ไขข้อผิดพลาดได้ 1 บิต 
    * **SEC-DED:** แก้ไขได้ 1 บิต และตรวจจับได้ว่าผิดพลาด 2 บิต

<a name="section53"></a>
## 5.3 DDR DRAM (แรมแบบ DDR)
* **SDRAM (Synchronous DRAM):** ทำงานสัมพันธ์กับสัญญาณนาฬิกา (Clock) ของระบบ ทำให้ Processor ไม่ต้องรอข้อมูลนานเกินไป
* **DDR (Double Data Rate):** * เพิ่มความเร็วในการส่งข้อมูลโดยส่งทั้งช่วง **ขาขึ้น (Rising edge)** และ **ขาลง (Falling edge)** ของสัญญาณนาฬิกา
  * มีการพัฒนาตั้งแต่ DDR1 จนถึง DDR4 โดยเพิ่มขนาด **Prefetch Buffer** และลดระดับแรงดันไฟฟ้าลง 

<a name="section54"></a>
## 5.4 Flash Memory (หน่วยความจำแฟลช)
* **คุณสมบัติ:** เป็นหน่วยความจำแบบ Non-volatile (ข้อมูลไม่หายเมื่อไม่มีไฟ) ที่อยู่กึ่งกลางระหว่าง EPROM และ EEPROM ในด้านราคาและการทำงาน 
* **ประเภทของ Flash:**
    * **NOR Flash:** รองรับการเข้าถึงข้อมูลแบบสุ่ม (Random access) เหมาะสำหรับเก็บ Program Code
    * **NAND Flash:** มีความหนาแน่นข้อมูลสูงกว่า ลบและเขียนเป็นบล็อก (Block-level) เหมาะสำหรับที่เก็บข้อมูลความจุสูง

<a name="section55"></a>
## 5.5 Newer Nonvolatile Solid-State Memory Technologies
เทคโนโลยีใหม่ที่พยายามรวมความเร็วของ RAM และความถาวรของ ROM เข้าด้วยกัน:
* **STT-RAM (Spin-Transfer Torque RAM):** ใช้ทิศทางการหมุนของอิเล็กตรอนในการเก็บข้อมูล
* **PCRAM (Phase-Change RAM):** ใช้การเปลี่ยนสถานะของวัสดุระหว่างผลึกและอสัณฐาน
* **ReRAM (Resistive RAM):** ใช้การเปลี่ยนความต้านทานไฟฟ้าของวัสดุฉนวน 

<a name="section56"></a>
## 5.6 Key Terms, Review Questions, and Problems
รายการคำศัพท์ที่สรุปจากเนื้อหาทั้งหมดในบท Internal Memory เพื่อใช้ในการทบทวน:

* **Dynamic RAM (DRAM):** หน่วยความจำที่เก็บข้อมูลในรูปของประจุไฟฟ้าในตัวเก็บประจุ ต้องมีการ **Refresh** ข้อมูลตลอดเวลาเนื่องจากประจุรั่วไหลได้
* **Static RAM (SRAM):** หน่วยความจำที่ใช้ลอจิกเกตแบบ Flip-flop ไม่ต้องรีเฟรชข้อมูล มีความเร็วสูงและราคาสูงกว่า DRAM
* **Read-Only Memory (ROM):** หน่วยความจำแบบ **Nonvolatile** ที่ข้อมูลไม่หายเมื่อไม่มีไฟเลี้ยง ใช้เก็บข้อมูลถาวรที่ไม่ต้องการการแก้ไข
* **Programmable ROM (PROM):** ROM ที่ผู้ใช้สามารถเขียนข้อมูลลงไปได้เองเพียงครั้งเดียว
* **Erasable PROM (EPROM):** หน่วยความจำที่ลบข้อมูลเดิมออกได้โดยการฉายแสง **UV** เพื่อนำกลับมาเขียนใหม่
* **Electrically Erasable PROM (EEPROM):** หน่วยความจำที่ลบและเขียนข้อมูลใหม่ได้ด้วยสัญญาณไฟฟ้าในระดับ Byte (สะดวกกว่า EPROM)
* **Flash Memory:** เทคโนโลยีหน่วยความจำแบบ Nonvolatile ที่ลบข้อมูลได้รวดเร็วในระดับ **Block** นิยมใช้ใน SSD และ USB Drive
* **Hard Failure:** ความเสียหายถาวรทางกายภาพของหน่วยความจำ (เช่น ชิปเสีย)
* **Soft Error:** ข้อผิดพลาดชั่วคราวที่เกิดจากสัญญาณรบกวนหรือไฟฟ้า โดยที่ตัวอุปกรณ์ไม่ได้เสียหายถาวร
* **Hamming Code:** รหัสตรวจแก้ข้อผิดพลาด (Error-Correcting Code) ที่ใช้ Check bits ในการระบุตำแหน่งบิตที่ผิด
* **SEC-DED (Single-Error-Correcting, Double-Error-Detecting):** ความสามารถในการแก้ไขข้อผิดพลาด 1 บิต และตรวจจับข้อผิดพลาด 2 บิตได้พร้อมกัน
* **SDRAM (Synchronous DRAM):** DRAM ที่ทำงานสอดคล้องกับจังหวะสัญญาณนาฬิกา (System Clock)
* **DDR (Double Data Rate):** เทคโนโลยีที่ส่งข้อมูลได้ทั้งขาขึ้นและขาลงของสัญญาณนาฬิกา ทำให้ความเร็วเพิ่มเป็น 2 เท่า
* **NAND Flash:** Flash Memory ที่มีความหนาแน่นสูง ลบข้อมูลเป็นบล็อก เหมาะสำหรับเก็บข้อมูลจำนวนมาก
* **NOR Flash:** Flash Memory ที่เข้าถึงข้อมูลแบบสุ่มได้เร็ว เหมาะสำหรับเก็บโปรแกรมคำสั่ง (Code Storage)
  
| ประเภทหน่วยความจำ | การลบข้อมูล | กลไกการเขียน | ความคงอยู่ (Volatility) | การใช้งานหลัก |
| :--- | :--- | :--- | :--- | :--- |
| **RAM (DRAM/SRAM)** | ไฟฟ้า (Byte) | ไฟฟ้า | **Volatile** (ข้อมูลหาย) | Main Memory / Cache |
| **ROM / PROM** | ทำไม่ได้ | Masks / ไฟฟ้า | **Nonvolatile** | Microprogramming |
| **EPROM** | แสง UV (Chip) | ไฟฟ้า | **Nonvolatile** | Read-mostly applications |
| **EEPROM** | ไฟฟ้า (Byte) | ไฟฟ้า | **Nonvolatile** | Read-mostly applications |
| **Flash Memory** | ไฟฟ้า (Block) | ไฟฟ้า | **Nonvolatile** | SSD, USB, Firmware |
