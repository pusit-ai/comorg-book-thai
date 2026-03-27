# บทที่ 4: Cache Memory (หน่วยความจำแคช)

## สารบัญ

- [4.1 ภาพรวมระบบหน่วยความจำคอมพิวเตอร์](#41-ภาพรวมระบบหน่วยความจำคอมพิวเตอร์)
  - [คุณลักษณะของระบบหน่วยความจำ](#คุณลักษณะของระบบหน่วยความจำ)
  - [ลำดับชั้นของหน่วยความจำ](#ลำดับชั้นของหน่วยความจำ)
- [4.2 หลักการของหน่วยความจำแคช](#42-หลักการของหน่วยความจำแคช)
- [4.3 องค์ประกอบของการออกแบบแคช](#43-องค์ประกอบของการออกแบบแคช)
  - [ที่อยู่แคช](#ที่อยู่แคช)
  - [ขนาดแคช](#ขนาดแคช)
  - [ฟังก์ชันการแมป](#ฟังก์ชันการแมป)
  - [อัลกอริทึมการแทนที่](#อัลกอริทึมการแทนที่)
  - [นโยบายการเขียน](#นโยบายการเขียน)
  - [ขนาดบรรทัด](#ขนาดบรรทัด)
  - [จำนวนแคช](#จำนวนแคช)
- [4.4 การจัดการแคชของ Pentium 4](#44-การจัดการแคชของ-pentium-4)
- [4.5 คำสำคัญ คำถามทบทวน และโจทย์ปัญหา](#45-คำสำคัญ-คำถามทบทวน-และโจทย์ปัญหา)
- [ภาคผนวก 4A: คุณลักษณะด้านประสิทธิภาพของหน่วยความจำสองระดับ](#ภาคผนวก-4a-คุณลักษณะด้านประสิทธิภาพของหน่วยความจำสองระดับ)

---

## วัตถุประสงค์การเรียนรู้

หลังจากศึกษาบทนี้แล้ว นักศึกษาควรสามารถ:

1. นำเสนอภาพรวมของคุณลักษณะหลักของระบบหน่วยความจำคอมพิวเตอร์ และการใช้ลำดับชั้นของหน่วยความจำ
2. อธิบายแนวคิดพื้นฐานและจุดประสงค์ของหน่วยความจำแคช
3. อภิปรายองค์ประกอบสำคัญของการออกแบบแคช
4. แยกแยะความแตกต่างระหว่าง Direct Mapping, Associative Mapping และ Set-Associative Mapping
5. อธิบายเหตุผลของการใช้แคชหลายระดับ
6. เข้าใจผลกระทบด้านประสิทธิภาพของหน่วยความจำหลายระดับ

---

## 4.1 ภาพรวมระบบหน่วยความจำคอมพิวเตอร์

แม้ว่าจะดูเรียบง่ายในแนวคิด แต่หน่วยความจำคอมพิวเตอร์มีความหลากหลายในด้านประเภท เทคโนโลยี การจัดโครงสร้าง ประสิทธิภาพ และราคามากที่สุดในบรรดาคุณลักษณะต่าง ๆ ของระบบคอมพิวเตอร์ ไม่มีเทคโนโลยีเดียวที่ดีที่สุดในการตอบสนองความต้องการหน่วยความจำของระบบคอมพิวเตอร์ ด้วยเหตุนี้ ระบบคอมพิวเตอร์ทั่วไปจึงติดตั้งลำดับชั้นของระบบย่อยหน่วยความจำ บางส่วนอยู่ภายในระบบ (เข้าถึงได้โดยตรงโดยโปรเซสเซอร์) และบางส่วนอยู่ภายนอก (เข้าถึงโดยโปรเซสเซอร์ผ่านโมดูล I/O)

บทนี้และบทถัดไปมุ่งเน้นไปที่องค์ประกอบหน่วยความจำภายใน ในขณะที่บทที่ 6 จะเน้นหน่วยความจำภายนอก ในการเริ่มต้น ส่วนแรกจะพิจารณาคุณลักษณะสำคัญของหน่วยความจำคอมพิวเตอร์ ส่วนที่เหลือของบทนี้จะพิจารณาองค์ประกอบสำคัญของระบบคอมพิวเตอร์สมัยใหม่ทั้งหมด นั่นคือ หน่วยความจำแคช

---

### คุณลักษณะของระบบหน่วยความจำ

เรื่องหน่วยความจำคอมพิวเตอร์ที่ซับซ้อนจะจัดการได้ง่ายขึ้น หากเราจำแนกระบบหน่วยความจำตามคุณลักษณะสำคัญ

**ตาราง 4.1 คุณลักษณะสำคัญของระบบหน่วยความจำคอมพิวเตอร์**

| Location (ตำแหน่ง) | Performance (ประสิทธิภาพ) |
|---|---|
| Internal เช่น processor registers, cache, main memory | Access time |
| External เช่น optical disks, magnetic disks, tapes | Cycle time |
| | Transfer rate |

| Capacity (ความจุ) | Physical Type (ประเภททางกายภาพ) |
|---|---|
| Number of words | Semiconductor |
| Number of bytes | Magnetic |
| | Optical |

| Unit of Transfer (หน่วยการถ่ายโอน) | Magneto-optical |
|---|---|
| Word | Physical Characteristics (คุณสมบัติทางกายภาพ) |
| Block | Volatile/Nonvolatile |
| | Erasable/Nonerasable |

| Access Method (วิธีการเข้าถึง) | Organization (การจัดโครงสร้าง) |
|---|---|
| Sequential | Memory modules |
| Direct | |
| Random | |
| Associative | |

---

#### Location (ตำแหน่ง)

คำว่า **location** หมายถึงหน่วยความจำที่อยู่ภายในหรือภายนอกคอมพิวเตอร์ หน่วยความจำภายในมักถูกเรียกว่า **main memory** แต่ยังมีหน่วยความจำภายในรูปแบบอื่น โปรเซสเซอร์ต้องการหน่วยความจำเฉพาะในรูปของ **register** นอกจากนี้ ส่วนควบคุมของโปรเซสเซอร์อาจต้องการหน่วยความจำภายในของตัวเองด้วย **แคช** เป็นหน่วยความจำภายในอีกรูปแบบหนึ่ง หน่วยความจำภายนอกประกอบด้วยอุปกรณ์จัดเก็บข้อมูลแบบ peripheral เช่น disk และ tape ซึ่งโปรเซสเซอร์เข้าถึงได้ผ่าน I/O controller

---

#### Capacity (ความจุ)

คุณลักษณะที่ชัดเจนอย่างหนึ่งของหน่วยความจำคือ **ความจุ** สำหรับหน่วยความจำภายใน มักแสดงเป็น byte (1 byte = 8 bits) หรือ word ความยาว word ที่พบทั่วไปคือ 8, 16 และ 32 บิต ความจุของหน่วยความจำภายนอกมักแสดงเป็น byte

---

#### Unit of Transfer (หน่วยการถ่ายโอน)

แนวคิดที่เกี่ยวข้องคือ **unit of transfer** สำหรับหน่วยความจำภายใน unit of transfer เท่ากับจำนวนสายไฟฟ้าที่เข้าและออกจากโมดูลหน่วยความจำ ซึ่งอาจเท่ากับความยาว word แต่มักใหญ่กว่า เช่น 64, 128 หรือ 256 บิต

สามแนวคิดที่เกี่ยวข้องสำหรับหน่วยความจำภายใน:

- **Word**: หน่วยการจัดโครงสร้างของหน่วยความจำที่ "เป็นธรรมชาติ" ขนาด word โดยทั่วไปเท่ากับจำนวนบิตที่ใช้แทนจำนวนเต็มและความยาว instruction ตัวอย่างเช่น CRAY C90 มีความยาว word 64 บิตแต่ใช้การแทนค่าจำนวนเต็ม 46 บิต สถาปัตยกรรม Intel x86 มีความยาว instruction หลากหลายแบบ และมีขนาด word 32 บิต

- **Addressable units**: ในบางระบบ หน่วยที่ระบุที่อยู่ได้คือ word แต่หลายระบบอนุญาตให้ระบุที่อยู่ระดับ byte ความสัมพันธ์ระหว่างความยาว A บิตของที่อยู่และจำนวน N ของหน่วยที่ระบุที่อยู่ได้คือ:
$$2^A = N$$

- **Unit of transfer**: สำหรับ main memory คือจำนวนบิตที่อ่านออกหรือเขียนเข้าหน่วยความจำในแต่ละครั้ง สำหรับหน่วยความจำภายนอก ข้อมูลมักถ่ายโอนเป็นหน่วยที่ใหญ่กว่า word มาก เรียกว่า **block**

---

#### Access Method (วิธีการเข้าถึง)

- **Sequential access**: หน่วยความจำถูกจัดเป็นหน่วยข้อมูลที่เรียกว่า record การเข้าถึงต้องทำตามลำดับ linear ที่กำหนด เวลาในการเข้าถึง record ใด ๆ จึงแปรผัน ตัวอย่าง: tape unit

- **Direct access**: เช่นเดียวกับ sequential access แต่ละ block หรือ record มีที่อยู่เฉพาะตามตำแหน่งทางกายภาพ การเข้าถึงทำโดยเข้าถึงตรงไปยังบริเวณใกล้เคียงก่อน แล้วค้นหาตามลำดับ เวลาการเข้าถึงยังคงแปรผัน ตัวอย่าง: disk unit

- **Random access**: แต่ละตำแหน่งที่ระบุที่อยู่ได้ใน memory มีกลไกระบุที่อยู่เฉพาะที่เชื่อมต่อไฟฟ้าแบบ unique เวลาในการเข้าถึงตำแหน่งที่กำหนดเป็นอิสระจากลำดับการเข้าถึงก่อนหน้าและคงที่ ตัวอย่าง: main memory และบางระบบ cache

- **Associative**: เป็น random access ประเภทหนึ่งที่ช่วยให้สามารถเปรียบเทียบตำแหน่งบิตที่ต้องการใน word สำหรับการจับคู่ที่กำหนด และทำสิ่งนี้กับทุก word พร้อมกัน word จะถูกค้นหาตามส่วนหนึ่งของเนื้อหา ไม่ใช่ที่อยู่ ตัวอย่าง: cache memory อาจใช้ associative access

---

#### Performance (ประสิทธิภาพ)

พารามิเตอร์ประสิทธิภาพสามตัว:

1. **Access time (latency)**: สำหรับ random-access memory คือเวลาที่ใช้ในการอ่านหรือเขียน กล่าวคือเวลาตั้งแต่ที่อยู่ถูกส่งไปยังหน่วยความจำจนถึงข้อมูลถูกจัดเก็บหรือพร้อมใช้งาน สำหรับ non-random-access memory คือเวลาที่ใช้ในการวางกลไกอ่าน-เขียนไปยังตำแหน่งที่ต้องการ

2. **Memory cycle time**: แนวคิดนี้ใช้กับ random-access memory เป็นหลัก ประกอบด้วย access time บวกกับเวลาเพิ่มเติมที่จำเป็นก่อนที่จะเริ่มการเข้าถึงครั้งที่สองได้ โปรดสังเกตว่า memory cycle time เกี่ยวข้องกับ system bus ไม่ใช่โปรเซสเซอร์

3. **Transfer rate**: คืออัตราที่ข้อมูลสามารถถ่ายโอนเข้าหรือออกจากหน่วยความจำ สำหรับ random-access memory เท่ากับ 1/(cycle time) สำหรับ non-random-access memory:

$$T_n = T_A + \frac{n}{R} \quad (4.1)$$

โดยที่:
- $T_n$ = เวลาเฉลี่ยในการอ่านหรือเขียน n บิต
- $T_A$ = เวลาเข้าถึงเฉลี่ย
- $n$ = จำนวนบิต
- $R$ = Transfer rate เป็น bits per second (bps)

---

#### Physical Type (ประเภททางกายภาพ)

ประเภทที่พบมากที่สุดในปัจจุบัน:
- Semiconductor memory
- Magnetic surface memory (ใช้สำหรับ disk และ tape)
- Optical และ magneto-optical

---

#### Physical Characteristics (คุณสมบัติทางกายภาพ)

- **Volatile memory**: ข้อมูลจะสูญหายตามธรรมชาติหรือหายไปเมื่อปิดไฟ
- **Nonvolatile memory**: ข้อมูลที่บันทึกแล้วจะคงอยู่โดยไม่เสื่อมสภาพจนกว่าจะตั้งใจเปลี่ยน ไม่จำเป็นต้องมีไฟเลี้ยงเพื่อเก็บข้อมูล ตัวอย่าง: Magnetic-surface memory
- **Nonerasable memory**: ไม่สามารถแก้ไขได้ ยกเว้นการทำลายหน่วยเก็บ Semiconductor memory ประเภทนี้เรียกว่า **read-only memory (ROM)**

---

### ลำดับชั้นของหน่วยความจำ

ข้อจำกัดในการออกแบบหน่วยความจำคอมพิวเตอร์สรุปได้ด้วยสามคำถาม: มากแค่ไหน? เร็วแค่ไหน? แพงแค่ไหน?

ในสเปกตรัมของเทคโนโลยีต่าง ๆ ความสัมพันธ์ต่อไปนี้เป็นจริง:
- เวลาเข้าถึงเร็วขึ้น → ราคาต่อบิตสูงขึ้น
- ความจุมากขึ้น → ราคาต่อบิตลดลง
- ความจุมากขึ้น → เวลาเข้าถึงช้าลง

ทางออกคือการใช้ **memory hierarchy** เมื่อลงมาตามลำดับชั้น สิ่งต่อไปนี้เกิดขึ้น:
- a. ราคาต่อบิตลดลง
- b. ความจุเพิ่มขึ้น
- c. เวลาเข้าถึงช้าลง
- d. ความถี่ที่โปรเซสเซอร์เข้าถึงหน่วยความจำลดลง

```
           ┌─────────────┐
           │  Registers  │
           └──────┬──────┘
           ┌──────▼──────┐    ← Inboard memory
           │    Cache    │
           └──────┬──────┘
           ┌──────▼──────┐
           │ Main Memory │
           └──────┬──────┘
           ┌──────▼──────┐    ← Outboard storage
           │ Magnetic    │
           │ disk        │
           │ CD-ROM      │
           │ CD-RW       │
           │ DVD-RW      │
           │ DVD-RAM     │
           │ Blu-Ray     │
           └──────┬──────┘
           ┌──────▼──────┐    ← Off-line storage
           │ Magnetic    │
           │ tape        │
           └─────────────┘
```
**รูปที่ 4.1 The Memory Hierarchy**

กุญแจสำคัญสู่ความสำเร็จของการจัดองค์กรนี้คือข้อ (d): ความถี่ในการเข้าถึงที่ลดลง พื้นฐานของความถูกต้องของเงื่อนไขนี้คือหลักการที่เรียกว่า **locality of reference** ในระหว่างการทำงานของโปรแกรม การอ้างอิงหน่วยความจำโดยโปรเซสเซอร์มีแนวโน้มที่จะกระจุกตัว โปรแกรมทั่วไปมี iterative loop และ subroutine จำนวนหนึ่ง เมื่อเข้าสู่ loop หรือ subroutine จะมีการอ้างอิงซ้ำ ๆ ไปยังชุด instruction เล็ก ๆ

---

#### ตัวอย่าง 4.1

สมมติว่าโปรเซสเซอร์มีหน่วยความจำสองระดับ:
- **Level 1**: มี 1,000 word, access time = 0.01 μs
- **Level 2**: มี 100,000 word, access time = 0.1 μs

สมมติว่า 95% ของการเข้าถึงหน่วยความจำพบใน level 1 เวลาเฉลี่ยในการเข้าถึง word:

$$(0.95)(0.01\ \mu s) + (0.05)(0.01\ \mu s + 0.1\ \mu s) = 0.0095 + 0.0055 = 0.015\ \mu s$$

เวลาเข้าถึงเฉลี่ยใกล้เคียงกับ 0.01 μs มากกว่า 0.1 μs ตามที่ต้องการ

---

การใช้หน่วยความจำสามรูปแบบ (register, cache, main memory) โดยทั่วไปเป็น volatile และใช้เทคโนโลยี semiconductor ข้อมูลถูกจัดเก็บอย่างถาวรมากขึ้นในอุปกรณ์จัดเก็บข้อมูลมวลภายนอก ซึ่งที่พบบ่อยที่สุดคือ hard disk และสื่อแบบถอดออกได้

#### Disk Cache

ส่วนหนึ่งของ main memory สามารถใช้เป็น buffer เพื่อเก็บข้อมูลชั่วคราวที่จะถ่ายโอนออกไปยัง disk เทคนิคนี้เรียกว่า **disk cache** ปรับปรุงประสิทธิภาพสองวิธี:
1. การเขียนลง Disk ถูกรวมกลุ่ม แทนที่จะถ่ายโอนข้อมูลจำนวนเล็กน้อยหลายครั้ง ถ่ายโอนข้อมูลจำนวนมากน้อยครั้ง ซึ่งปรับปรุงประสิทธิภาพ disk และลดการมีส่วนร่วมของโปรเซสเซอร์
2. ข้อมูลบางส่วนที่กำหนดให้เขียนออกอาจถูกอ้างอิงโดยโปรแกรมก่อนที่จะ dump ลง disk ครั้งถัดไป ในกรณีนั้น ข้อมูลจะถูกเรียกคืนอย่างรวดเร็วจาก software cache

---

## 4.2 หลักการของหน่วยความจำแคช

หน่วยความจำแคชถูกออกแบบมาเพื่อรวมเวลาการเข้าถึงหน่วยความจำของหน่วยความจำที่แพงและความเร็วสูงเข้ากับขนาดหน่วยความจำขนาดใหญ่ของหน่วยความจำที่ถูกกว่าและช้ากว่า

แนวคิดการทำงาน:
- มี main memory ขนาดค่อนข้างใหญ่และช้า พร้อมกับ cache memory ขนาดเล็กและเร็ว
- แคชมีสำเนาบางส่วนของ main memory
- เมื่อโปรเซสเซอร์พยายามอ่าน word จากหน่วยความจำ จะมีการตรวจสอบว่า word นั้นอยู่ใน cache หรือไม่
  - **Cache hit**: ถ้ามี word จะถูกส่งให้โปรเซสเซอร์
  - **Cache miss**: ถ้าไม่มี block ของ main memory ที่ประกอบด้วยจำนวน word คงที่จะถูกอ่านเข้า cache แล้ว word จะถูกส่งให้โปรเซสเซอร์

เนื่องจากปรากฏการณ์ **locality of reference** เมื่อ block ของข้อมูลถูกดึงเข้า cache มีความเป็นไปได้สูงที่จะมีการอ้างอิงในอนาคตไปยังตำแหน่งหน่วยความจำเดียวกันหรือ word อื่น ๆ ใน block นั้น

---

### โครงสร้างของระบบ Cache/Main Memory

Main memory มี word ที่สามารถระบุที่อยู่ได้ถึง $2^n$ word โดยแต่ละ word มีที่อยู่ n บิตเฉพาะ สำหรับวัตถุประสงค์การแมป หน่วยความจำนี้ถือว่าประกอบด้วย block ความยาวคงที่จำนวนหนึ่ง โดยแต่ละ block มี K word นั่นคือมี $M = 2^n/K$ block ใน main memory

**แคชประกอบด้วย m block เรียกว่า line** โดย:
- แต่ละ line มี K word บวก tag สองสามบิต
- แต่ละ line ยังมี control bits เช่น bit เพื่อระบุว่า line ถูกแก้ไขตั้งแต่โหลดเข้า cache หรือไม่
- **Line size**: ความยาวของ line ไม่รวม tag และ control bits
- จำนวน line น้อยกว่าจำนวน main memory block มาก (m << M)
- แต่ละ line มี **tag** ที่ระบุว่า block ใดกำลังถูกจัดเก็บอยู่

### Read Operation

เมื่อเกิด **cache hit**: data และ address buffer จะถูก disable และการสื่อสารจะอยู่ระหว่างโปรเซสเซอร์กับ cache เท่านั้น โดยไม่มี system bus traffic

เมื่อเกิด **cache miss**: ที่อยู่ที่ต้องการจะถูกโหลดบน system bus และข้อมูลจะถูกส่งกลับผ่าน data buffer ไปยังทั้ง cache และโปรเซสเซอร์

---

## 4.3 องค์ประกอบของการออกแบบแคช

**ตาราง 4.2 องค์ประกอบของการออกแบบแคช**

| Cache Addresses | Write Policy |
|---|---|
| Logical | Write through |
| Physical | Write back |

| Cache Size | Line Size |
|---|---|
| | Number of Caches |

| Mapping Function | Single or two level |
|---|---|
| Direct | Unified or split |
| Associative | |
| Set associative | |

| Replacement Algorithm | |
|---|---|
| Least recently used (LRU) | |
| First in first out (FIFO) | |
| Least frequently used (LFU) | |
| Random | |

---

### ที่อยู่แคช

โปรเซสเซอร์ที่ไม่ใช่ embedded เกือบทั้งหมดรองรับ **virtual memory** ซึ่งเป็น facility ที่อนุญาตให้โปรแกรมระบุที่อยู่หน่วยความจำจากมุมมองเชิง logic โดยไม่คำนึงถึงปริมาณ main memory ที่มีจริง Hardware **memory management unit (MMU)** จะแปลที่อยู่ virtual แต่ละตัวเป็น physical address ใน main memory

#### Logical Cache (Virtual Cache)
- จัดเก็บข้อมูลโดยใช้ virtual address
- โปรเซสเซอร์เข้าถึง cache โดยตรงโดยไม่ผ่าน MMU
- **ข้อได้เปรียบ**: cache access speed เร็วกว่า physical cache เนื่องจาก cache สามารถตอบสนองก่อนที่ MMU จะทำการแปลที่อยู่
- **ข้อเสีย**: virtual memory system ส่วนใหญ่จัดหา virtual memory address space เดียวกันให้กับแต่ละแอปพลิเคชัน ดังนั้น cache memory จะต้อง flush อย่างสมบูรณ์กับการสลับ application context แต่ละครั้ง

#### Physical Cache
- จัดเก็บข้อมูลโดยใช้ physical address ของ main memory

---

### ขนาดแคช

เราต้องการให้ขนาดของ cache:
- **เล็กพอ** ที่ราคาต่อบิตเฉลี่ยรวมใกล้เคียงกับ main memory เพียงอย่างเดียว
- **ใหญ่พอ** ที่เวลาเข้าถึงเฉลี่ยรวมใกล้เคียงกับ cache เพียงอย่างเดียว

ยิ่ง cache ใหญ่ขึ้น ยิ่งมี gate จำนวนมากขึ้นในการระบุที่อยู่ cache ส่งผลให้ cache ขนาดใหญ่มีแนวโน้มช้ากว่า cache ขนาดเล็กเล็กน้อย

**ตาราง 4.3 ขนาดแคชของโปรเซสเซอร์บางตัว**

| Processor | Type | Year | L1 Cache | L2 Cache | L3 Cache |
|---|---|---|---|---|---|
| IBM 360/85 | Mainframe | 1968 | 16–32 kB | — | — |
| PDP-11/70 | Minicomputer | 1975 | 1 kB | — | — |
| VAX 11/780 | Minicomputer | 1978 | 16 kB | — | — |
| IBM 3033 | Mainframe | 1978 | 64 kB | — | — |
| IBM 3090 | Mainframe | 1985 | 128–256 kB | — | — |
| Intel 80486 | PC | 1989 | 8 kB | — | — |
| Pentium | PC | 1993 | 8 kB/8 kB | 256–512 kB | — |
| PowerPC 601 | PC | 1993 | 32 kB | — | — |
| PowerPC 620 | PC | 1996 | 32 kB/32 kB | — | — |
| PowerPC G4 | PC/server | 1999 | 32 kB/32 kB | 256 kB to 1 MB | 2 MB |
| IBM S/390 G6 | Mainframe | 1999 | 256 kB | 8 MB | — |
| Pentium 4 | PC/server | 2000 | 8 kB/8 kB | 256 kB | — |
| IBM SP | High-end server | 2000 | 64 kB/32 kB | 8 MB | — |
| CRAY MTA | Supercomputer | 2000 | 8 kB | 2 MB | — |
| Itanium | PC/server | 2001 | 16 kB/16 kB | 96 kB | 4 MB |
| Itanium 2 | PC/server | 2002 | 32 kB | 256 kB | 6 MB |
| IBM POWER5 | High-end server | 2003 | 64 kB | 1.9 MB | 36 MB |
| CRAY XD-1 | Supercomputer | 2004 | 64 kB/64 kB | 1 MB | — |
| IBM POWER6 | PC/server | 2007 | 64 kB/64 kB | 4 MB | 32 MB |
| IBM z10 | Mainframe | 2008 | 64 kB/128 kB | 3 MB | 24–48 MB |
| Intel Core i7 EE 990 | Workstation/server | 2011 | 6×32 kB/32 kB | 1.5 MB | 12 MB |
| IBM zEnterprise 196 | Mainframe/server | 2011 | 24×64 kB/128 kB | 24×1.5 MB | 24 MB L3, 192 MB L4 |

> **หมายเหตุ**: ค่าสองตัวที่คั่นด้วย slash หมายถึง instruction cache และ data cache

---

### ฟังก์ชันการแมป

เนื่องจากมี cache line น้อยกว่า main memory block จำเป็นต้องมีอัลกอริทึมในการแมป main memory block เข้า cache line สามเทคนิคที่สามารถใช้ได้:

#### ตัวอย่าง 4.2 (ข้อมูลพื้นฐาน)

สำหรับทั้งสามกรณี:
- Cache สามารถเก็บข้อมูลได้ **64 kB**
- ข้อมูลถูกถ่ายโอนเป็น block ขนาด **4 byte** → cache ถูกจัดเป็น 16K = 2¹⁴ line ของ 4 byte แต่ละ line
- Main memory มีขนาด **16 MB** โดยแต่ละ byte ระบุที่อยู่ได้โดยตรงด้วย 24-bit address (2²⁴ = 16M)
- Main memory ประกอบด้วย **4M block** ของ 4 byte แต่ละ block

---

#### Direct Mapping

เทคนิคที่ง่ายที่สุด จะแมปแต่ละ block ของ main memory ไปยัง cache line ที่เป็นไปได้เพียงบรรทัดเดียว:

$$i = j \bmod m$$

โดยที่:
- $i$ = หมายเลข cache line
- $j$ = หมายเลข main memory block
- $m$ = จำนวน line ใน cache

**โครงสร้างที่อยู่สำหรับ Direct Mapping:**

| Tag (s - r bits) | Line (r bits) | Word (w bits) |
|---|---|---|

สรุป:
- ความยาวที่อยู่ = (s + w) bits
- จำนวน addressable unit = $2^{s+w}$ word หรือ byte
- Block size = line size = $2^w$ word หรือ byte
- จำนวน block ใน main memory = $2^s$
- จำนวน line ใน cache = m = $2^r$
- ขนาด cache = $2^{r+w}$ word หรือ byte
- ขนาด tag = (s - r) bits

##### ตัวอย่าง 4.2a: Direct Mapping

ในตัวอย่างนี้ m = 16K = 2¹⁴ และ i = j mod 2¹⁴

| Cache Line | Starting Memory Address of Block |
|---|---|
| 0 | 000000, 010000, …, FF0000 |
| 1 | 000004, 010004, …, FF0004 |
| ⋮ | ⋮ |
| 2¹⁴ - 1 | 00FFFC, 01FFFC, …, FFFFFC |

**Read operation**: ระบบ cache จะได้รับ 24-bit address โดย:
1. 14-bit line number ใช้เป็น index เข้าสู่ cache เพื่อเข้าถึง line เฉพาะ
2. ถ้า 8-bit tag number ตรงกับ tag number ที่จัดเก็บอยู่ใน line นั้น → cache hit
3. 2-bit word number จะถูกใช้เพื่อเลือกหนึ่งใน 4 byte ใน line นั้น
4. ถ้าไม่ตรงกัน → 22-bit tag-plus-line field ใช้ดึง block จาก main memory

**ข้อเสีย**: มี cache location คงที่สำหรับ block ที่กำหนดใด ๆ ดังนั้น ถ้าโปรแกรมอ้างอิง word ซ้ำ ๆ จาก block ต่างสองตัวที่แมปไปยัง line เดียวกัน block เหล่านั้นจะถูกสลับใน cache ตลอดเวลา และ hit ratio จะต่ำ (เรียกว่า **thrashing**)

---

#### Associative Mapping

Associative mapping เอาชนะข้อเสียของ direct mapping โดยอนุญาตให้แต่ละ main memory block โหลดเข้า **line ใดก็ได้** ของ cache

cache control logic ตีความที่อยู่ memory เพียงแค่เป็นสองฟิลด์:

| Tag (s bits) | Word (w bits) |
|---|---|

เพื่อพิจารณาว่า block อยู่ใน cache หรือไม่ cache control logic ต้องตรวจสอบ tag ของทุก line พร้อมกัน

สรุป:
- ความยาวที่อยู่ = (s + w) bits
- จำนวน addressable unit = $2^{s+w}$ word หรือ byte
- Block size = line size = $2^w$ word หรือ byte
- จำนวน block ใน main memory = $2^s$
- จำนวน line ใน cache = ไม่แน่นอน
- ขนาด tag = s bits

**ข้อเสีย**: วงจรซับซ้อนที่จำเป็นในการตรวจสอบ tag ของ cache line ทั้งหมดพร้อมกัน

##### ตัวอย่าง 4.2b: Associative Mapping

ที่อยู่ main memory ประกอบด้วย **22-bit tag** และ **2-bit byte number**

ตัวอย่าง: 24-bit hexadecimal address `16339C` มี 22-bit tag `058CE7`
```
Memory address: 0001 0110 0011 0011 1001 1100 (binary) = 1 6 3 3 9 C (hex)
Tag (22 bits):  00 0101 1000 1100 1110 0111 (binary) = 0 5 8 C E 7 (hex)
```

---

#### Set-Associative Mapping

Set-associative mapping เป็นการประนีประนอมที่แสดงจุดแข็งของทั้ง direct และ associative approach

$$m = v \times k$$
$$i = j \bmod v$$

โดยที่:
- $i$ = หมายเลข cache set
- $j$ = หมายเลข main memory block
- $m$ = จำนวน line ใน cache
- $v$ = จำนวน set
- $k$ = จำนวน line ในแต่ละ set

เรียกว่า **k-way set-associative mapping**

**โครงสร้างที่อยู่สำหรับ Set-Associative Mapping:**

| Tag (s - d bits) | Set (d bits) | Word (w bits) |
|---|---|---|

สรุป:
- ความยาวที่อยู่ = (s + w) bits
- จำนวน addressable unit = $2^{s+w}$ word หรือ byte
- Block size = line size = $2^w$ word หรือ byte
- จำนวน block ใน main memory = $2^s$
- จำนวน line ใน set = k
- จำนวน set = v = $2^d$
- จำนวน line ใน cache = m = $k \times 2^d$
- ขนาด cache = $k \times 2^{d+w}$ word หรือ byte
- ขนาด tag = (s - d) bits

ในกรณีสุดขีด:
- v = m, k = 1 → ลดเป็น **direct mapping**
- v = 1, k = m → ลดเป็น **associative mapping**

การใช้สอง line ต่อ set (k = 2) เป็น set-associative organization ที่พบบ่อยที่สุด ปรับปรุง hit ratio เหนือ direct mapping อย่างมีนัยสำคัญ Four-way set associative (k = 4) ทำการปรับปรุงเพิ่มเติมเล็กน้อย

##### ตัวอย่าง 4.2c: Two-Way Set-Associative Mapping

13-bit set number ระบุ set เฉพาะของสอง line ภายใน cache

ตัวอย่าง: block 000000, 008000, …, FF8000 ของ main memory แมปเข้า cache set 0 โดย block ใดก็ได้เหล่านั้นสามารถโหลดเข้า line ใดของสอง line ใน set ก็ได้

---

### อัลกอริทึมการแทนที่

เมื่อ cache ถูกเติมเต็มแล้ว เมื่อ block ใหม่ถูกนำเข้า cache หนึ่งใน block ที่มีอยู่ต้องถูกแทนที่ สำหรับ direct mapping ไม่มีทางเลือก แต่สำหรับ associative และ set-associative จำเป็นต้องมี replacement algorithm

#### 1. Least Recently Used (LRU) — **แนะนำ**
แทนที่ block ที่อยู่ใน cache นานที่สุดโดยไม่มีการอ้างอิง สำหรับ two-way set associative แต่ละ line มี USE bit:
- เมื่อ line ถูกอ้างอิง → USE bit ตั้งเป็น 1 และ USE bit ของ line อื่นใน set นั้นตั้งเป็น 0
- เมื่อ block จะถูกอ่านเข้า set → ใช้ line ที่มี USE bit เป็น 0

เนื่องจากเราสมมติว่าตำแหน่งหน่วยความจำที่ใช้ล่าสุดมีแนวโน้มที่จะถูกอ้างอิงมากกว่า LRU ควรให้ hit ratio ที่ดีที่สุด **LRU เป็น replacement algorithm ที่นิยมที่สุด**

#### 2. First-In-First-Out (FIFO)
แทนที่ block ที่อยู่ใน cache นานที่สุด นำไปใช้ได้ง่ายด้วยเทคนิค round-robin หรือ circular buffer

#### 3. Least Frequently Used (LFU)
แทนที่ block ที่มีการอ้างอิงน้อยที่สุด นำไปใช้ได้โดยการเชื่อม counter กับแต่ละ line

#### 4. Random
เลือก line แบบสุ่มจาก candidate line การศึกษาจำลองแสดงให้เห็นว่า random replacement ให้ประสิทธิภาพด้อยกว่าอัลกอริทึมที่อิงการใช้งานเพียงเล็กน้อย

---

### นโยบายการเขียน

เมื่อ block ที่อยู่ใน cache จะถูกแทนที่ มีสองกรณี:
1. ถ้า block เดิมใน cache **ไม่ได้ถูกแก้ไข** → เขียนทับด้วย block ใหม่ได้เลย
2. ถ้ามีการเขียนอย่างน้อยหนึ่งครั้งบน word ใน line → ต้องอัปเดต main memory ก่อน

#### Write Through
การเขียนทั้งหมดจะถูกส่งไปยัง main memory และ cache พร้อมกัน ทำให้ main memory ถูกต้องเสมอ

- **ข้อดี**: processor-cache module อื่นใดก็ตามสามารถตรวจสอบ traffic ไปยัง main memory เพื่อรักษาความสอดคล้องภายใน cache ของตัวเอง
- **ข้อเสีย**: สร้าง memory traffic จำนวนมากและอาจสร้าง bottleneck

#### Write Back
การอัปเดตจะทำเฉพาะใน cache เมื่อเกิดการอัปเดต **dirty bit** ที่เชื่อมกับ line จะถูกตั้งค่า จากนั้น เมื่อ block ถูกแทนที่ จะถูกเขียนกลับ main memory ถ้าและต่อเมื่อ dirty bit ถูกตั้งค่า

- **ข้อดี**: ลดการเขียนหน่วยความจำให้น้อยที่สุด
- **ข้อเสีย**: ส่วนของ main memory อาจ invalid, การเข้าถึงโดย I/O module จึงสามารถอนุญาตได้เฉพาะผ่าน cache

> **หมายเหตุ**: ประสบการณ์แสดงให้เห็นว่าเปอร์เซ็นต์ของการอ้างอิงหน่วยความจำที่เป็นการเขียนอยู่ที่ประมาณ 15% แต่สำหรับ HPC application ตัวเลขนี้อาจเข้าใกล้ 33% (vector-vector multiplication) และสูงถึง 50% (matrix transposition)

---

#### ตัวอย่าง 4.3: Write-Back vs Write-Through

พิจารณา cache ที่มี line size 32 bytes และ main memory ที่ต้องใช้ 30 ns ในการถ่ายโอน 4-byte word

- **Write-back**: แต่ละ dirty line จะถูกเขียนกลับครั้งเดียวเมื่อ swap-out → ใช้เวลา 8 × 30 = **240 ns**
- **Write-through**: แต่ละการอัปเดต line ต้องเขียน word ออกไปยัง main memory → ใช้เวลา **30 ns ต่อครั้ง**

**สรุป**: ถ้า line เฉลี่ยที่ถูกเขียนอย่างน้อยหนึ่งครั้งถูกเขียนมากกว่า **8 ครั้ง** ก่อน swap out แล้ว write-back จะมีประสิทธิภาพมากกว่า

---

#### Cache Coherency

ในองค์กรแบบ bus ที่มีหลายโปรเซสเซอร์มี cache และ main memory ร่วมกัน ถ้าข้อมูลใน cache ตัวหนึ่งถูกแก้ไข นี่จะ invalidate ไม่เพียงแค่ word ที่สอดคล้องกันใน main memory แต่ยัง word เดียวกันใน cache อื่น ๆ ด้วย แนวทางที่เป็นไปได้:

1. **Bus watching with write through**: cache controller แต่ละตัวตรวจสอบ address line เพื่อตรวจจับ write operation ไปยัง memory โดย bus master อื่น ถ้า master อื่นเขียนไปยังตำแหน่งใน shared memory ที่อยู่ใน cache memory ด้วย cache controller จะ invalidate cache entry นั้น

2. **Hardware transparency**: ใช้ hardware เพิ่มเติมเพื่อให้แน่ใจว่าการอัปเดตทั้งหมดไปยัง main memory ผ่าน cache สะท้อนใน cache ทั้งหมด

3. **Noncacheable memory**: เฉพาะส่วนหนึ่งของ main memory ที่ใช้ร่วมกันโดยโปรเซสเซอร์มากกว่าหนึ่งตัว และถูกกำหนดให้เป็น noncacheable ในระบบดังกล่าว การเข้าถึง shared memory ทั้งหมดเป็น cache miss

---

### ขนาดบรรทัด

เมื่อขนาด block เพิ่มขึ้นจากขนาดเล็กมากไปยังขนาดใหญ่ขึ้น hit ratio จะเพิ่มขึ้นในตอนแรกเนื่องจากหลักการ locality แต่เมื่อขนาด block ใหญ่เกินไป hit ratio จะเริ่มลดลง เนื่องจากผลสองอย่าง:

1. **Block ที่ใหญ่กว่าลดจำนวน block ที่พอดีใน cache**: เนื่องจาก block fetch แต่ละครั้งเขียนทับเนื้อหา cache เดิม จำนวน block น้อยส่งผลให้ข้อมูลถูกเขียนทับไม่นานหลังจากถูกดึงมา

2. **แต่ละ word เพิ่มเติมอยู่ห่างจาก word ที่ต้องการมากขึ้น**: มีแนวโน้มน้อยลงที่จะถูกต้องการในอนาคตอันใกล้

> **ขนาดที่ดี**: ขนาดตั้งแต่ **8 ถึง 64 byte** ดูเหมือนจะใกล้เคียงกับจุดที่เหมาะสม สำหรับระบบ HPC ขนาด cache line **64 และ 128 byte** ถูกใช้บ่อยที่สุด

---

### จำนวนแคช

#### Multilevel Caches

เมื่อความหนาแน่น logic เพิ่มขึ้น จึงเป็นไปได้ที่จะมี cache อยู่บนชิปเดียวกับโปรเซสเซอร์ เรียกว่า **on-chip cache**

**ข้อดีของ on-chip cache**:
- ลด external bus activity ของโปรเซสเซอร์
- เร่งเวลาประมวลผลและเพิ่มประสิทธิภาพรวมของระบบ
- เมื่อพบ instruction หรือข้อมูลใน on-chip cache การเข้าถึง bus จะถูกขจัดออก

**Two-level cache**: มี level 1 (L1) ภายใน และ external cache ที่กำหนดเป็น level 2 (L2)

เหตุผลในการรวม L2 cache:
- ถ้าไม่มี L2 cache และโปรเซสเซอร์ส่ง access request ไปยังตำแหน่งที่ไม่อยู่ใน L1 cache → โปรเซสเซอร์จะต้องเข้าถึง DRAM หรือ ROM memory ผ่าน bus ซึ่งช้า
- ถ้าใช้ L2 SRAM cache ข้อมูลที่ขาดหายมักสามารถดึงมาได้อย่างรวดเร็ว

ด้วยพื้นที่ on-chip ที่มีเพิ่มขึ้น microprocessor ร่วมสมัยส่วนใหญ่ได้ย้าย L2 cache เข้าสู่ processor chip และเพิ่ม L3 cache นอกจากนี้ระบบขนาดใหญ่ เช่น IBM mainframe zEnterprise systems ปัจจุบันรวม 3 ระดับ cache บนชิปและระดับที่สี่ของ cache ที่ใช้ร่วมกันในหลายชิป

---

#### Unified versus Split Caches

**Unified cache**: cache เดียวที่ใช้จัดเก็บการอ้างอิงทั้ง data และ instruction

**ข้อได้เปรียบของ Unified cache**:
1. สำหรับขนาด cache ที่กำหนด มี hit rate สูงกว่า split cache เนื่องจากสมดุล load ระหว่าง instruction fetch กับ data fetch โดยอัตโนมัติ
2. ต้องออกแบบและนำไปใช้เพียง cache เดียว

**Split cache**: แยกเป็น instruction cache และ data cache

**ข้อได้เปรียบของ Split cache**:
- ขจัด contention สำหรับ cache ระหว่าง instruction fetch/decode unit กับ execution unit ซึ่งสำคัญในการออกแบบที่พึ่งพาการ pipeline ของ instruction
- ป้องกันการที่ instruction prefetcher ถูกบล็อกเมื่อ execution unit เข้าถึงหน่วยความจำเพื่อ load และ store data

> **แนวโน้ม**: ปัจจุบันนิยมใช้ **split cache ที่ L1** และ **unified cache สำหรับระดับสูงกว่า** โดยเฉพาะสำหรับ superscalar machine

---

## 4.4 การจัดการแคชของ Pentium 4

**ตาราง 4.4 วิวัฒนาการของ Intel Cache**

| ปัญหา | แนวทางแก้ไข | โปรเซสเซอร์แรกที่ใช้ |
|---|---|---|
| External memory ช้ากว่า system bus | เพิ่ม external cache โดยใช้เทคโนโลยีหน่วยความจำที่เร็วกว่า | 386 |
| ความเร็วโปรเซสเซอร์ที่เพิ่มขึ้นทำให้ external bus กลายเป็น bottleneck สำหรับการเข้าถึง cache | ย้าย external cache เข้าชิปทำงานที่ความเร็วเดียวกับโปรเซสเซอร์ | 486 |
| Internal cache เล็กเกินไป เนื่องจากพื้นที่บนชิปจำกัด | เพิ่ม external L2 cache โดยใช้เทคโนโลยีที่เร็วกว่า main memory | 486 |
| Contention เกิดขึ้นเมื่อทั้ง Instruction Prefetcher และ Execution Unit ต้องการเข้าถึง cache พร้อมกัน | สร้าง data และ instruction cache แยกกัน | Pentium |
| ความเร็วโปรเซสเซอร์ที่เพิ่มขึ้นทำให้ external bus กลายเป็น bottleneck สำหรับการเข้าถึง L2 cache | สร้าง back-side bus แยกต่างหากที่ทำงานเร็วกว่า main (front-side) external bus | Pentium Pro |
| | ย้าย L2 cache เข้า processor chip | Pentium II |
| แอปพลิเคชันบางตัวจัดการ database ขนาดใหญ่ on-chip cache เล็กเกินไป | เพิ่ม external L3 cache | Pentium III |
| | ย้าย L3 cache เข้าชิป | Pentium 4 |

### Pentium 4 Organization

Processor core ประกอบด้วยส่วนประกอบหลักสี่อย่าง:

1. **Fetch/decode unit**: ดึง program instruction ตามลำดับจาก L2 cache แปลงเป็นชุด micro-operation และจัดเก็บผลลัพธ์ใน L1 instruction cache

2. **Out-of-order execution logic**: กำหนดตารางการประมวลผล micro-operation ตาม data dependency และความพร้อมของ resource micro-operation อาจถูกกำหนดตารางสำหรับการประมวลผลในลำดับที่แตกต่างจากที่ดึงมาจาก instruction stream

3. **Execution units**: ประมวลผล micro-operation โดยดึงข้อมูลที่ต้องการจาก L1 data cache และจัดเก็บผลลัพธ์ชั่วคราวใน register

4. **Memory subsystem**: รวมถึง L2 และ L3 cache และ system bus ใช้เข้าถึง main memory เมื่อ L1 และ L2 cache เกิด cache miss

### ข้อแตกต่างสำคัญของ Pentium 4

Pentium 4 **instruction cache วางอยู่ระหว่าง instruction decode logic กับ execution core** (ต่างจากโปรเซสเซอร์อื่นส่วนใหญ่)

เหตุผล: Pentium process แปล Pentium machine instruction เป็น RISC-like instruction ที่เรียบง่ายที่เรียกว่า **micro-operation** การใช้ micro-operation ที่เรียบง่ายและมีความยาวคงที่ช่วยให้ใช้เทคนิค superscalar pipelining และ scheduling ที่ปรับปรุงประสิทธิภาพ

### Cache Specifications ของ Pentium 4

- **L1 data cache**: ขนาด 16 kB, line size 64 bytes, four-way set-associative, ใช้ **write-back policy** (กำหนดค่าได้ dynamic เพื่อรองรับ write-through)
- **L2 และ L3 cache**: eight-way set-associative มี line size 128 bytes

### Control Bits สำหรับ L1 Data Cache

**ตาราง 4.5 โหมดการทำงานของ Pentium 4 Cache**

| CD | NW | Cache Fills | Write Throughs | Invalidates |
|---|---|---|---|---|
| 0 | 0 | Enabled | Enabled | Enabled |
| 1 | 0 | Disabled | Enabled | Enabled |
| 1 | 1 | Disabled | Disabled | Disabled |

> **หมายเหตุ**: CD = 0; NW = 1 เป็น combination ที่ไม่ถูกต้อง

**Instruction สำหรับควบคุม data cache**:
- `INVD`: invalidates (flushes) internal cache memory
- `WBINVD`: เขียนกลับและ invalidate internal cache แล้วเขียนกลับและ invalidate external cache

---

## 4.5 คำสำคัญ คำถามทบทวน และโจทย์ปัญหา

### คำสำคัญ

`access time` · `associative mapping` · `cache hit` · `cache line` · `cache memory` · `cache miss` · `cache set` · `data cache` · `direct access` · `direct mapping` · `high-performance computing (HPC)` · `hit` · `hit ratio` · `instruction cache` · `L1 cache` · `L2 cache` · `L3 cache` · `line` · `locality` · `logical cache` · `memory hierarchy` · `miss` · `multilevel cache` · `physical address` · `physical cache` · `random access` · `replacement algorithm` · `secondary memory` · `sequential access` · `set-associative mapping` · `spatial locality` · `split cache` · `tag` · `temporal locality` · `unified cache` · `virtual address` · `virtual cache` · `write back` · `write through`

---

### คำถามทบทวน

**4.1** ความแตกต่างระหว่าง sequential access, direct access และ random access คืออะไร?

**4.2** ความสัมพันธ์ทั่วไประหว่าง access time, ราคาหน่วยความจำ และความจุคืออะไร?

**4.3** หลักการ locality เกี่ยวข้องกับการใช้หน่วยความจำหลายระดับอย่างไร?

**4.4** ความแตกต่างระหว่าง direct mapping, associative mapping และ set-associative mapping คืออะไร?

**4.5** สำหรับ direct-mapped cache ที่อยู่ main memory ถูกมองว่าประกอบด้วยสามฟิลด์ แสดงรายการและนิยามสามฟิลด์นั้น

**4.6** สำหรับ associative cache ที่อยู่ main memory ถูกมองว่าประกอบด้วยสองฟิลด์ แสดงรายการและนิยามสองฟิลด์นั้น

**4.7** สำหรับ set-associative cache ที่อยู่ main memory ถูกมองว่าประกอบด้วยสามฟิลด์ แสดงรายการและนิยามสามฟิลด์นั้น

**4.8** ความแตกต่างระหว่าง spatial locality และ temporal locality คืออะไร?

**4.9** โดยทั่วไป กลยุทธ์สำหรับการใช้ประโยชน์จาก spatial locality และ temporal locality คืออะไร?

---

### โจทย์ปัญหา

**4.1** set-associative cache ประกอบด้วย 64 line หรือ slot แบ่งเป็น four-line set Main memory มี 4K block ของ 128 word แต่ละ block แสดงรูปแบบที่อยู่ main memory

**4.2** two-way set-associative cache มี line ขนาด 16 byte และขนาดรวม 8 kB Main memory ขนาด 64 MB สามารถระบุที่อยู่ได้ระดับ byte แสดงรูปแบบที่อยู่ main memory

**4.3** สำหรับที่อยู่ main memory แบบ hexadecimal 111111, 666666, BBBBBB แสดงข้อมูลต่อไปนี้ในรูปแบบ hexadecimal:

- a. ค่า Tag, Line และ Word สำหรับ direct-mapped cache โดยใช้รูปแบบของรูปที่ 4.10
- b. ค่า Tag และ Word สำหรับ associative cache โดยใช้รูปแบบของรูปที่ 4.12
- c. ค่า Tag, Set และ Word สำหรับ two-way set-associative cache โดยใช้รูปแบบของรูปที่ 4.15

**4.4** แสดงค่าต่อไปนี้:

- a. สำหรับตัวอย่าง direct cache: ความยาวที่อยู่, จำนวน addressable unit, ขนาด block, จำนวน block ใน main memory, จำนวน line ใน cache, ขนาด tag
- b. สำหรับตัวอย่าง associative cache: ความยาวที่อยู่, จำนวน addressable unit, ขนาด block, จำนวน block ใน main memory, จำนวน line ใน cache, ขนาด tag
- c. สำหรับตัวอย่าง two-way set-associative cache: ความยาวที่อยู่, จำนวน addressable unit, ขนาด block, จำนวน block ใน main memory, จำนวน line ใน set, จำนวน set, จำนวน line ใน cache, ขนาด tag

**4.5** พิจารณา 32-bit microprocessor ที่มี on-chip 16-kB four-way set-associative cache สมมติว่า cache มี line size สี่ 32-bit word วาด block diagram ของ cache นี้ word จากตำแหน่ง memory ABCDE8F8 แมปไปยังที่ใดใน cache?

**4.6** กำหนด specification ต่อไปนี้สำหรับ external cache memory: four-way set associative; line size สอง 16-bit word; สามารถรองรับ 4K 32-bit word รวมจาก main memory; ใช้กับ 16-bit processor ที่ออก 24-bit address ออกแบบโครงสร้าง cache

**4.7** Intel 80486 มี on-chip, unified cache มีขนาด 8 kB และมี four-way set-associative organization และ block length สี่ 32-bit word cache ถูกจัดเป็น 128 set วาด diagram ที่เรียบง่ายของ cache

**4.8** พิจารณาเครื่องที่มี byte addressable main memory ขนาด 2¹⁶ byte และ block size 8 byte สมมติว่าใช้ direct mapped cache ประกอบด้วย 32 line

- a. 16-bit memory address ถูกแบ่งอย่างไรเป็น tag, line number และ byte number?
- b. byte ที่มีที่อยู่แต่ละตัวต่อไปนี้จะถูกจัดเก็บใน line ใด?
  - `0001 0001 0001 1011`
  - `1100 0011 0011 0100`
  - `1101 0000 0001 1101`
  - `1010 1010 1010 1010`
- c. สมมติว่า byte ที่มีที่อยู่ `0001 1010 0001 1010` ถูกจัดเก็บใน cache ที่อยู่ของ byte อื่นที่จัดเก็บพร้อมกับมันคืออะไร?
- d. จำนวน byte ทั้งหมดของ memory ที่สามารถจัดเก็บใน cache คือเท่าไหร่?
- e. ทำไม tag ถึงถูกจัดเก็บใน cache ด้วย?

**4.9** สำหรับ on-chip cache Intel 80486 ใช้ replacement algorithm ที่เรียกว่า **pseudo least recently used** เชื่อมกับแต่ละ 128 set ของสี่ line มีสามบิต B0, B1 และ B2

- a. ระบุวิธีที่บิต B0, B1 และ B2 ถูกตั้งค่า
- b. แสดงว่า algorithm ของ 80486 เป็น approximation ของ LRU algorithm จริง ๆ
- c. แสดงให้เห็นว่า LRU algorithm จริงต้องการ 6 bit ต่อ set

**4.10** set-associative cache มี block size สี่ 16-bit word และ set size 2 cache สามารถรองรับรวม 4096 word main memory ขนาดที่สามารถ cache ได้คือ 64K × 32 bit ออกแบบโครงสร้าง cache

**4.11** พิจารณาระบบหน่วยความจำที่ใช้ 32-bit address ระบุที่อยู่ระดับ byte บวกกับ cache ที่ใช้ line size 64 byte

- a. สมมติว่า direct mapped cache มี tag field ในที่อยู่ 20 bit แสดงรูปแบบที่อยู่
- b. สมมติว่า associative cache แสดงรูปแบบที่อยู่
- c. สมมติว่า four-way set-associative cache มี tag field ในที่อยู่ 9 bit แสดงรูปแบบที่อยู่

**4.12** พิจารณาคอมพิวเตอร์ที่มี main memory รวม 1 MB; word size 1 byte; block size 16 byte; และ cache size 64 kB

- a. สำหรับที่อยู่ F0010, 01234 และ CABBE ให้ tag, cache line address และ word offset สำหรับ direct-mapped cache
- b. ให้สองที่อยู่ main memory ใด ๆ ที่มี tag ต่างกันซึ่งแมปไปยัง cache slot เดียวกัน
- c. สำหรับที่อยู่ F0010 และ CABBE ให้ค่า tag และ offset สำหรับ fully-associative cache
- d. สำหรับที่อยู่ F0010 และ CABBE ให้ค่า tag, cache set และ offset สำหรับ two-way set-associative cache

**4.13** อธิบายเทคนิคง่าย ๆ สำหรับการนำ LRU replacement algorithm ไปใช้ใน four-way set-associative cache

**4.14** พิจารณาตัวอย่าง 4.3 อีกครั้ง คำตอบเปลี่ยนไปอย่างไรถ้า main memory ใช้ block transfer capability ที่มี first-word access time 30 ns และ access time 5 ns สำหรับแต่ละ word ต่อไป?

**4.15** พิจารณา code ต่อไปนี้:

```c
for (i = 0; i < 20; i++)
  for (j = 0; j < 10; j++)
    a[i] = a[i] * j
```

- a. ให้ตัวอย่างหนึ่งของ spatial locality ใน code
- b. ให้ตัวอย่างหนึ่งของ temporal locality ใน code

**4.16** Generalize สมการ (4.2) และ (4.3) ในภาคผนวก 4A ให้กับ N-level memory hierarchy

**4.17** ระบบคอมพิวเตอร์มี main memory 32K 16-bit word และ 4K word cache แบ่งเป็น four-line set ที่มี 64 word ต่อ line สมมติว่า cache ว่างในตอนแรก โปรเซสเซอร์ดึง word จากตำแหน่ง 0, 1, 2, …, 4351 ตามลำดับ แล้วทำซ้ำลำดับการดึงนี้อีกเก้าครั้ง cache เร็วกว่า main memory 10 เท่า ประมาณการปรับปรุงที่เกิดจากการใช้ cache สมมติว่าใช้ LRU policy

**4.18** พิจารณา cache ที่มี 4 line ของ 16 byte แต่ละตัว Main memory แบ่งเป็น block ขนาด 16 byte แต่ละ block พิจารณาโปรแกรมที่เข้าถึงหน่วยความจำในลำดับที่อยู่ต่อไปนี้: ครั้งเดียว: 63 ถึง 70; วนซ้ำสิบครั้ง: 15 ถึง 32; 80 ถึง 95

- a. สมมติว่า cache ถูกจัดเป็น direct mapped คำนวณ hit ratio
- b. สมมติว่า cache ถูกจัดเป็น two-way set associative คำนวณ hit ratio โดยใช้ LRU replacement scheme

**4.19** ระบบคอมพิวเตอร์มี parameter ต่อไปนี้:
$$T_c = 100\ \text{ns}, \quad C_c = 10^{-4}\ \text{$/bit}$$
$$T_m = 1200\ \text{ns}, \quad C_m = 10^{-5}\ \text{$/bit}$$

- a. ราคาของ main memory 1 MB คือเท่าไหร่?
- b. ราคาของ main memory 1 MB โดยใช้เทคโนโลยี cache memory คือเท่าไหร่?
- c. ถ้า effective access time มากกว่า cache access time 10% hit ratio H คือเท่าไหร่?

**4.20**
- a. พิจารณา L1 cache ที่มี access time 1 ns และ hit ratio H = 0.95 ถ้าเปลี่ยนการออกแบบเพื่อเพิ่ม H เป็น 0.97 แต่เพิ่ม access time เป็น 1.5 ns เงื่อนไขใดบ้างที่ต้องเป็นจริงเพื่อให้การเปลี่ยนแปลงนี้ส่งผลให้ประสิทธิภาพดีขึ้น?
- b. อธิบายว่าทำไมผลลัพธ์นี้จึงสมเหตุสมผลในเชิงสัญชาตญาณ

**4.21** พิจารณา single-level cache ที่มี access time 2.5 ns, line size 64 byte และ hit ratio H = 0.95 Main memory ใช้ block transfer capability ที่มี first-word (4 byte) access time 50 ns และ access time 5 ns สำหรับแต่ละ word ต่อไป

- a. access time เมื่อเกิด cache miss คือเท่าไหร่?
- b. สมมติว่าการเพิ่ม line size เป็น 128 byte ทำให้ H เพิ่มเป็น 0.97 สิ่งนี้ลด average memory access time หรือไม่?

**4.22** คอมพิวเตอร์มี cache, main memory และ disk ที่ใช้สำหรับ virtual memory:
- word ที่อ้างอิงอยู่ใน cache: ใช้เวลา **20 ns**
- อยู่ใน main memory แต่ไม่ใน cache: ต้องใช้ 60 ns เพื่อโหลดเข้า cache
- ไม่อยู่ใน main memory: ต้องใช้ 12 ms เพื่อดึงจาก disk ตามด้วย 60 ns เพื่อ copy ไปยัง cache
- cache hit ratio = 0.9, main memory hit ratio = 0.6

เวลาเฉลี่ยที่ต้องการในการเข้าถึง word บนระบบนี้คือเท่าไหร่?

**4.23** พิจารณา cache ที่มี line size 64 byte สมมติว่าโดยเฉลี่ย 30% ของ line ใน cache เป็น dirty word ประกอบด้วย 8 byte

- a. สมมติว่ามี miss rate 3% (0.97 hit ratio) คำนวณปริมาณ main memory traffic เป็น byte ต่อ instruction สำหรับทั้ง write-through และ write-back policy
- b. ทำซ้ำส่วน a สำหรับ rate 5%
- c. ทำซ้ำส่วน a สำหรับ rate 7%
- d. ข้อสรุปใดที่คุณสามารถดึงออกมาจากผลลัพธ์เหล่านี้?

**4.24** บน Motorola 68020 microprocessor การเข้าถึง cache ใช้เวลา clock cycle สองรอบ การเข้าถึง data จาก main memory ผ่าน bus ไปยังโปรเซสเซอร์ใช้เวลา clock cycle สามรอบ

- a. คำนวณความยาวที่มีประสิทธิภาพของ memory cycle โดยกำหนด hit ratio 0.9 และ clocking rate 16.67 MHz
- b. ทำซ้ำการคำนวณโดยสมมติว่ามีการแทรก wait state สองตัวของ one cycle แต่ละตัวต่อ memory cycle

**4.25** สมมติว่าโปรเซสเซอร์มี memory cycle time 300 ns และ instruction processing rate 1 MIPS โดยเฉลี่ยแต่ละ instruction ต้องการ bus memory cycle หนึ่งรอบสำหรับ instruction fetch และหนึ่งรอบสำหรับ operand

- a. คำนวณ utilization ของ bus โดยโปรเซสเซอร์
- b. สมมติว่าโปรเซสเซอร์ติดตั้ง instruction cache และ hit ratio ที่เกี่ยวข้องคือ 0.5 ระบุผลกระทบต่อ bus utilization

**4.26** ประสิทธิภาพของระบบ single-level cache สำหรับ read operation:
$$T_a = T_c + (1-H)T_m$$

- a. นิยาม $T_b$ = เวลาในการถ่ายโอน line และ W = เศษส่วนของ write reference ปรับสมการเพื่อรองรับทั้ง write และ read โดยใช้ write-through policy
- b. นิยาม $W_b$ เป็นความน่าจะเป็นที่ line ใน cache ถูกแก้ไข ให้สมการสำหรับ $T_a$ สำหรับ write-back policy

**4.27** สำหรับระบบที่มี cache สองระดับ นิยาม:
- $T_{c_1}$ = first-level cache access time
- $T_{c_2}$ = second-level cache access time
- $T_m$ = memory access time
- $H_1$ = first-level cache hit ratio
- $H_2$ = combined first/second level cache hit ratio

ให้สมการสำหรับ $T_a$ สำหรับ read operation

**4.28** สมมติว่ามีคุณลักษณะประสิทธิภาพต่อไปนี้บน cache read miss: หนึ่ง clock cycle เพื่อส่งที่อยู่ไปยัง main memory และสี่ clock cycle เพื่อเข้าถึง 32-bit word และถ่ายโอนไปยังโปรเซสเซอร์และ cache

- a. ถ้า cache line size เป็น one word miss penalty คือเท่าไหร่?
- b. miss penalty คือเท่าไหรถ้า cache line size เป็นสี่ word และมีการดำเนินการ multiple, nonburst transfer?
- c. miss penalty คือเท่าไหรถ้า cache line size เป็นสี่ word และดำเนินการ transfer โดยมีหนึ่ง clock cycle ต่อ word transfer?

**4.29** สำหรับการออกแบบ cache ของปัญหา 4.28 สมมติว่าการเพิ่ม line size จาก one word เป็นสี่ word ส่งผลให้ read miss rate ลดลงจาก 3.2% เป็น 1.1% average miss penalty เฉลี่ยในการ read ทั้งหมดสำหรับ line size ทั้งสองแบบคือเท่าไหร่?

---

## ภาคผนวก 4A: คุณลักษณะด้านประสิทธิภาพของหน่วยความจำสองระดับ

### Locality

พื้นฐานของข้อได้เปรียบด้านประสิทธิภาพของหน่วยความจำสองระดับคือหลักการที่เรียกว่า **locality of reference** หลักการนี้ระบุว่าการอ้างอิงหน่วยความจำมีแนวโน้มที่จะกระจุกตัว

**เหตุผลที่ locality สมเหตุสมผล**:

1. ยกเว้น branch และ call instruction (คิดเป็นเพียงเศษส่วนเล็กน้อยของ program instruction ทั้งหมด) การทำงานของโปรแกรมเป็น sequential

2. การมีลำดับยาวของ procedure call ที่ไม่ถูกขัดจังหวะตามด้วยลำดับ return ที่สอดคล้องกันเป็นเรื่องที่เกิดขึ้นน้อย โปรแกรมโดยทั่วไปจะถูกจำกัดอยู่ในช่วงความลึกของการเรียก procedure ที่แคบ

3. iterative construct ส่วนใหญ่ประกอบด้วย instruction จำนวนน้อยที่ทำซ้ำหลายครั้ง

4. ในโปรแกรมหลายตัว การคำนวณส่วนใหญ่เกี่ยวข้องกับการประมวลผล data structure เช่น array หรือลำดับ record

---

**ตาราง 4.6 คุณลักษณะของหน่วยความจำสองระดับ**

| | Main Memory Cache | Virtual Memory (paging) | Disk Cache |
|---|---|---|---|
| Typical access time ratios | 5:1 (main memory vs. cache) | 10⁶:1 (main memory vs. disk) | 10⁶:1 (main memory vs. disk) |
| Memory management system | Implemented by special hardware | Combination of hardware and system software | System software |
| Typical block or page size | 4 to 128 bytes (cache block) | 64 to 4096 bytes (virtual memory page) | 64 to 4096 bytes (disk block or pages) |
| Access of processor to second level | Direct access | Indirect access | Indirect access |

---

**ตาราง 4.7 ความถี่ Dynamic สัมพัทธ์ของการดำเนินการภาษาระดับสูง**

| Study | Pascal Scientific | FORTRAN Student | Pascal System | C System | SAL System |
|---|---|---|---|---|---|
| Assign | 74 | 67 | 45 | 38 | 42 |
| Loop | 4 | 3 | 5 | 3 | 4 |
| Call | 1 | 3 | 15 | 12 | 12 |
| IF | 20 | 11 | 29 | 43 | 36 |
| GOTO | 2 | 9 | — | 3 | — |
| Other | — | 7 | 6 | 1 | 6 |

---

#### Spatial Locality vs Temporal Locality

- **Spatial locality**: แนวโน้มของการทำงานที่เกี่ยวข้องกับตำแหน่งหน่วยความจำจำนวนหนึ่งที่กระจุกตัว สะท้อนแนวโน้มของโปรเซสเซอร์ในการเข้าถึง instruction ตามลำดับ ถูกใช้ประโยชน์โดยใช้ **cache block ขนาดใหญ่** และ **prefetching mechanism**

- **Temporal locality**: แนวโน้มของโปรเซสเซอร์ในการเข้าถึงตำแหน่งหน่วยความจำที่ใช้ล่าสุด เช่น เมื่อ iteration loop ถูกทำงาน ถูกใช้ประโยชน์โดยการเก็บค่า instruction และ data ที่ใช้ล่าสุดไว้ใน **cache memory** และ **cache hierarchy**

---

### การทำงานของหน่วยความจำสองระดับ

**upper-level memory (M1)**: เล็กกว่า เร็วกว่า และแพงกว่า (ต่อบิต)
**lower-level memory (M2)**: ใหญ่กว่า ช้ากว่า และถูกกว่า

เมื่อมีการอ้างอิงหน่วยความจำ:
1. พยายามเข้าถึง item ใน M1
2. ถ้าสำเร็จ → เข้าถึงได้อย่างรวดเร็ว
3. ถ้าไม่สำเร็จ → block ของตำแหน่งหน่วยความจำจะถูก copy จาก M2 ไปยัง M1 แล้วการเข้าถึงจะเกิดขึ้นผ่าน M1

---

### ประสิทธิภาพ

#### สมการ Average Access Time

$$T_s = H \times T_1 + (1-H) \times (T_1 + T_2) = T_1 + (1-H) \times T_2 \quad (4.2)$$

โดยที่:
- $T_s$ = เวลาการเข้าถึงเฉลี่ย (system)
- $T_1$ = access time ของ M1 (เช่น cache, disk cache)
- $T_2$ = access time ของ M2 (เช่น main memory, disk)
- $H$ = hit ratio (เศษส่วนของเวลาที่พบการอ้างอิงใน M1)

#### สมการ Average Cost

$$C_s = \frac{C_1 S_1 + C_2 S_2}{S_1 + S_2} \quad (4.3)$$

โดยที่:
- $C_s$ = ราคาต่อบิตเฉลี่ยสำหรับหน่วยความจำสองระดับรวม
- $C_1$ = ราคาต่อบิตเฉลี่ยของ upper-level memory M1
- $C_2$ = ราคาต่อบิตเฉลี่ยของ lower-level memory M2
- $S_1$ = ขนาดของ M1
- $S_2$ = ขนาดของ M2

เราต้องการ $C_s \approx C_2$ เมื่อกำหนดว่า $C_1 \gg C_2$ ต้องการ $S_1 < S_2$

#### Access Efficiency

$$\frac{T_1}{T_s} = \frac{1}{1 + (1-H)\frac{T_2}{T_1}} \quad (4.4)$$

ค่าทั่วไป:
- **On-chip cache**: $T_2/T_1$ ≈ 25 ถึง 50
- **Off-chip cache**: $T_2/T_1$ ≈ 5 ถึง 15
- **Main memory vs disk**: $T_2/T_1$ ≈ 1000

> **สรุป**: hit ratio ในช่วงใกล้ **0.9** ดูเหมือนจะต้องการเพื่อตอบสนองความต้องการด้านประสิทธิภาพ

---

#### ผลของ Locality ต่อ Hit Ratio

การศึกษาหลายชิ้นแสดงให้เห็นว่า cache ขนาดค่อนข้างเล็กจะให้ hit ratio เกิน **0.75** โดยไม่ขึ้นกับขนาดของ main memory cache ในช่วง **1K ถึง 128K word** โดยทั่วไปเพียงพอ ในขณะที่ main memory ในปัจจุบันโดยทั่วไปอยู่ในช่วง gigabyte

สรุป: ถ้า **M1 ขนาดสัมพัทธ์เล็ก** สามารถให้ **hit ratio สูง** ได้เนื่องจาก locality ส่งผลให้ **ราคาต่อบิตเฉลี่ยของหน่วยความจำสองระดับจะเข้าใกล้ค่าของ lower-level memory ที่ถูกกว่า**

---
