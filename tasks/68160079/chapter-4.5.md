# 4.5 Key Terms, Review Questions, and Problems

## ความหมายของหัวข้อนี้
หัวข้อ 4.5 ไม่ได้เป็นเนื้อหาใหม่เชิงทฤษฎีเหมือน 4.1–4.4 แต่เป็นส่วนสรุปท้ายบท เพื่อใช้

- ทบทวนคำศัพท์สำคัญ
- ตรวจสอบความเข้าใจ
- ฝึกคิดและฝึกแก้ปัญหาเกี่ยวกับ cache memory

## สิ่งที่ควรจำจากบทนี้
หัวข้อที่ 4.5 สะท้อนว่าแก่นของบทอยู่ที่เรื่องต่อไปนี้

### 1. ภาพรวมของระบบหน่วยความจำ
- หน่วยความจำมีหลายชนิด
- ต้องพิจารณาทั้งความจุ ความเร็ว วิธีเข้าถึง และราคา
- การออกแบบจริงต้องใช้ memory hierarchy

### 2. หลักการของ Cache Memory
- cache ช่วยลดเวลาเฉลี่ยในการเข้าถึงข้อมูล
- อาศัยหลัก locality of reference
- hit และ miss เป็นตัวชี้วัดสำคัญของประสิทธิภาพ

### 3. องค์ประกอบการออกแบบ Cache
- ที่อยู่ของ cache: logical / physical
- ขนาด cache
- mapping function
- replacement algorithm
- write policy
- line size
- จำนวนระดับของ cache

### 4. รูปแบบการแมปข้อมูล
- Direct mapping
- Associative mapping
- Set-associative mapping

ต้องเข้าใจความต่างเรื่อง
- ความง่ายของวงจร
- ความยืดหยุ่น
- โอกาสเกิด conflict miss
- ผลต่อ hit ratio

### 5. นโยบายแทนที่และการเขียนข้อมูล
- LRU, FIFO, LFU, Random
- Write through, Write back

### 6. ระบบ Cache หลายระดับ
- L1, L2, L3
- Unified กับ Split cache
- เหตุผลที่แยก instruction และ data cache

### 7. ตัวอย่างจริงจาก Pentium 4
- การใช้หลายระดับ cache
- การแยก L1 data cache กับ L1 instruction cache
- การเก็บ decoded micro-operations ใน instruction cache

## แนวทางใช้ไฟล์นี้ใน responsibility
ถ้าจะเอาไปใส่ responsibility หรือสรุปงาน สามารถใช้หัวข้อย่อแบบนี้ได้

### Responsibility Summary
- อธิบายลักษณะสำคัญของระบบหน่วยความจำและ memory hierarchy ได้
- อธิบายหลักการทำงานของ cache memory ได้
- เปรียบเทียบ direct, associative และ set-associative mapping ได้
- อธิบาย replacement algorithms และ write policies ได้
- สรุปโครงสร้าง cache หลายระดับและตัวอย่างจาก Pentium 4 ได้

## สรุปสั้นที่สุด
หัวข้อ 4.5 คือส่วนสำหรับ “เก็บบทเรียนทั้งหมดให้พร้อมทบทวน” โดยเน้นคำศัพท์ คำถามทบทวน และโจทย์ฝึกคิดจากเนื้อหาเรื่อง cache memory ทั้งบท
