# 4.3 Elements of Cache Design

## ภาพรวม
การออกแบบ cache ไม่ได้มีแค่เรื่อง “มี cache หรือไม่มี” แต่ต้องตัดสินใจหลายองค์ประกอบ ซึ่งมีผลต่อ

- ความเร็ว
- hit ratio
- ความซับซ้อนของฮาร์ดแวร์
- ต้นทุน

หัวข้อหลักของการออกแบบ ได้แก่

1. Cache addresses
2. Cache size
3. Mapping function
4. Replacement algorithm
5. Write policy
6. Line size
7. Number of caches

---

## 1) Cache Addresses
เมื่อระบบใช้ virtual memory จะมี 2 แนวทางหลัก

### Logical (Virtual) Cache
เก็บข้อมูลโดยใช้ virtual address

**ข้อดี**
- เร็ว เพราะ cache ตอบสนองได้ก่อน MMU แปล address

**ข้อเสีย**
- virtual address เดียวกันจากคนละ process อาจอ้างถึง physical address คนละตำแหน่ง
- ต้อง flush cache เมื่อ context switch หรือเพิ่มข้อมูลระบุ address space

### Physical Cache
เก็บข้อมูลโดยใช้ physical address

**ข้อดี**
- จัดการความถูกต้องของข้อมูลระหว่าง process ได้ง่ายกว่า

**ข้อเสีย**
- ช้ากว่า logical cache เพราะต้องรอ MMU แปล address ก่อน

---

## 2) Cache Size
ขนาด cache ต้องเลือกให้สมดุล

- เล็กเกินไป → hit ratio ต่ำ
- ใหญ่เกินไป → วงจรซับซ้อนขึ้น ใช้พื้นที่มากขึ้น และอาจช้าลงเล็กน้อย

ดังนั้นไม่มี “ขนาดที่ดีที่สุด” แบบตายตัว เพราะขึ้นอยู่กับ workload ด้วย

---

## 3) Mapping Function
เป็นวิธีจับคู่ block ใน main memory กับ line ใน cache

### 3.1 Direct Mapping
แต่ละ block ใน main memory จะไปลงได้ **เพียง line เดียวเท่านั้น**

**ข้อดี**
- ง่าย
- เร็ว
- วงจรไม่ซับซ้อน

**ข้อเสีย**
- ถ้า block ที่ใช้งานบ่อย 2 ตัว map มาชน line เดียวกัน จะเกิดการสลับเข้าออกบ่อย
- ปัญหานี้เรียกว่า **thrashing**

### 3.2 Associative Mapping
block ใด ๆ ใน main memory สามารถไปลง **line ไหนก็ได้** ใน cache

**ข้อดี**
- ยืดหยุ่นสูง
- ลดปัญหาชนกันของ block

**ข้อเสีย**
- ต้องเปรียบเทียบ tag กับทุก line พร้อมกัน
- วงจรซับซ้อนและมีต้นทุนสูง

### 3.3 Set-Associative Mapping
เป็นทางสายกลางระหว่าง direct และ associative

- cache ถูกแบ่งเป็นหลาย **sets**
- 1 block จะ map ไปได้ภายใน set ที่กำหนด
- แต่เลือก line ใดใน set นั้นก็ได้

ตัวอย่างที่นิยมคือ
- 2-way set associative
- 4-way set associative

**ข้อดี**
- hit ratio ดีกว่า direct mapping มาก
- ไม่ซับซ้อนเท่า fully associative

**แนวโน้มทั่วไป**
- 2-way ให้ผลดีชัดเจนเมื่อเทียบกับ direct
- 4-way ดีขึ้นอีกเล็กน้อย
- เพิ่ม way มากเกินไปมักได้ประโยชน์ไม่มากเมื่อเทียบกับความซับซ้อนที่เพิ่มขึ้น

---

## 4) Replacement Algorithm
เมื่อ cache เต็มแล้ว และต้องเอา block ใหม่เข้ามา ต้องมีนโยบายเลือกว่าจะไล่ block ไหนออก

### LRU (Least Recently Used)
เอา block ที่ไม่ได้ถูกใช้นานที่สุดออก

- มักให้ผลดี
- เป็นวิธีที่นิยมมาก

### FIFO (First In First Out)
เอา block ที่เข้ามาก่อนสุดออก

- ทำง่าย
- ไม่ได้ดูการใช้งานล่าสุดจริง

### LFU (Least Frequently Used)
เอา block ที่ถูกใช้น้อยที่สุดออก

- อิงจำนวนครั้งที่ถูกอ้างถึง

### Random
สุ่มเลือก line ที่จะออก

- ทำง่ายมาก
- ประสิทธิภาพด้อยกว่าแบบอิงการใช้งานเพียงเล็กน้อยในหลายกรณี

---

## 5) Write Policy
เวลามีการเขียนข้อมูลลง cache มี 2 แนวทางหลัก

### Write Through
เขียนทั้ง cache และ main memory พร้อมกัน

**ข้อดี**
- main memory ถูกต้องตลอด
- ง่ายต่อการคงความสอดคล้องของข้อมูล

**ข้อเสีย**
- เกิด traffic ไป main memory มาก
- อาจเป็นคอขวด

### Write Back
เขียนเฉพาะใน cache ก่อน แล้วค่อยเขียนกลับ main memory ตอน block ถูกแทนที่

**ข้อดี**
- ลดการเขียนลง main memory
- ประสิทธิภาพดีกว่าในหลายกรณี

**ข้อเสีย**
- main memory อาจไม่อัปเดตทันที
- ต้องมี dirty bit
- วงจรควบคุมซับซ้อนขึ้น

---

## 6) Line Size
Line size คือจำนวนข้อมูลที่เก็บใน 1 cache line

- line เล็กเกินไป → ใช้ประโยชน์จาก spatial locality ได้น้อย
- line ใหญ่เกินไป → เปลืองพื้นที่ cache และอาจโหลดข้อมูลที่ไม่จำเป็น

จึงต้องเลือกขนาดที่เหมาะสมกับรูปแบบการเข้าถึงข้อมูล

---

## 7) Number of Caches
ระบบสามารถมี cache ได้หลายแบบ เช่น

### Single-level / Two-level / Multi-level
- L1, L2, L3 ช่วยลดเวลาเฉลี่ยในการเข้าถึงหน่วยความจำ

### Unified vs Split Cache
**Unified cache**
- ใช้เก็บทั้ง instruction และ data ในที่เดียว
- ใช้พื้นที่ยืดหยุ่นกว่า

**Split cache**
- แยก instruction cache กับ data cache
- ลด contention ระหว่างการดึงคำสั่งกับการเข้าถึงข้อมูล
- นิยมใช้ที่ระดับ L1

---

## ใจความสำคัญของหัวข้อนี้
- การออกแบบ cache มีหลายตัวแปรและไม่มีคำตอบเดียวที่ดีที่สุด
- mapping function มีผลมากต่อ hit ratio และความซับซ้อนของระบบ
- set-associative เป็นแนวทางที่นิยมเพราะสมดุลระหว่างประสิทธิภาพกับต้นทุน
- replacement และ write policy ส่งผลโดยตรงต่อความเร็วและความถูกต้องของข้อมูล
- ระบบสมัยใหม่มักใช้หลายระดับ cache และนิยมแยก instruction/data ที่ L1
