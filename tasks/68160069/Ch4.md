# บทที่ 4: หน่วยความจำแคช (Cache Memory)
---

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
- [ภาคผนวก 4A คุณลักษณะด้านประสิทธิภาพของหน่วยความจำสองระดับ](#ภาคผนวก-4a)

---

## 🎯 วัตถุประสงค์การเรียนรู้

หลังจากศึกษาบทนี้แล้ว นักศึกษาควรสามารถ:

- นำเสนอภาพรวมของคุณลักษณะหลักของระบบหน่วยความจำคอมพิวเตอร์ และการใช้ลำดับชั้นของหน่วยความจำ
- อธิบายแนวคิดพื้นฐานและจุดประสงค์ของหน่วยความจำแคช
- อภิปรายองค์ประกอบสำคัญของการออกแบบแคช
- แยกแยะความแตกต่างระหว่าง Direct Mapping, Associative Mapping และ Set-Associative Mapping
- อธิบายเหตุผลของการใช้แคชหลายระดับ
- เข้าใจผลกระทบด้านประสิทธิภาพของหน่วยความจำหลายระดับ

---

## 4.1 ภาพรวมระบบหน่วยความจำคอมพิวเตอร์

แม้ว่าจะดูเรียบง่ายในแนวคิด แต่หน่วยความจำคอมพิวเตอร์มีความหลากหลายในด้านประเภท เทคโนโลยี การจัดโครงสร้าง ประสิทธิภาพ และราคามากที่สุดในบรรดาคุณลักษณะต่างๆ ของระบบคอมพิวเตอร์ ไม่มีเทคโนโลยีเดียวที่ดีที่สุดในการตอบสนองความต้องการหน่วยความจำ ด้วยเหตุนี้ ระบบคอมพิวเตอร์ทั่วไปจึงติดตั้ง **ลำดับชั้นของระบบย่อยหน่วยความจำ** บางส่วนอยู่ภายในระบบ และบางส่วนอยู่ภายนอก

---

### คุณลักษณะของระบบหน่วยความจำ

**ตาราง 4.1 คุณลักษณะสำคัญของระบบหน่วยความจำคอมพิวเตอร์**

| Location (ตำแหน่ง) | Performance (ประสิทธิภาพ) |
|---|---|
| Internal: processor registers, cache, main memory | Access time |
| External: optical disks, magnetic disks, tapes | Cycle time, Transfer rate |
| **Capacity (ความจุ)** | **Physical Type (ประเภททางกายภาพ)** |
| Number of words / bytes | Semiconductor, Magnetic, Optical, Magneto-optical |
| **Unit of Transfer** | **Physical Characteristics** |
| Word, Block | Volatile/Nonvolatile, Erasable/Nonerasable |
| **Access Method** | **Organization** |
| Sequential, Direct, Random, Associative | Memory modules |

#### วิธีการเข้าถึงหน่วยความจำ

| วิธีการ | ลักษณะ | ตัวอย่าง |
|--------|---------|---------|
| **Sequential access** | เข้าถึงตามลำดับ linear | Tape unit |
| **Direct access** | เข้าถึงบริเวณใกล้เคียงก่อน | Disk unit |
| **Random access** | เข้าถึงโดยตรงด้วยเวลาคงที่ | Main memory, Cache |
| **Associative** | ค้นหาตามเนื้อหา ไม่ใช่ที่อยู่ | Cache memory |

#### พารามิเตอร์ประสิทธิภาพ

- **Access time (latency):** เวลาตั้งแต่ส่งที่อยู่จนถึงข้อมูลพร้อมใช้
- **Memory cycle time:** Access time + เวลาเพิ่มเติมก่อนเริ่มการเข้าถึงครั้งถัดไป
- **Transfer rate:** อัตราการถ่ายโอนข้อมูล สำหรับ random-access = 1/(cycle time)

สำหรับ non-random-access memory:

$$T_n = T_A + \frac{n}{R}$$

โดยที่ $T_n$ = เวลาเฉลี่ยในการอ่าน/เขียน n บิต, $T_A$ = access time เฉลี่ย, $R$ = Transfer rate (bps)

---

### ลำดับชั้นของหน่วยความจำ

ข้อจำกัดในการออกแบบสรุปด้วยสามคำถาม: **มากแค่ไหน? เร็วแค่ไหน? แพงแค่ไหน?**

ความสัมพันธ์ระหว่างเทคโนโลยีต่างๆ:
- เวลาเข้าถึงเร็วขึ้น → ราคาต่อบิตสูงขึ้น
- ความจุมากขึ้น → ราคาต่อบิตลดลง
- ความจุมากขึ้น → เวลาเข้าถึงช้าลง

```
         ┌─────────────┐
         │  Registers  │  ← เร็วที่สุด แพงที่สุด
         └──────┬──────┘
         ┌──────▼──────┐
         │    Cache    │  ← Inboard memory
         └──────┬──────┘
         ┌──────▼──────┐
         │ Main Memory │
         └──────┬──────┘
         ┌──────▼──────┐
         │  Magnetic   │  ← Outboard storage
         │    Disk     │
         └──────┬──────┘
         ┌──────▼──────┐
         │  Magnetic   │  ← Off-line storage
         │    Tape     │
         └─────────────┘
```
**รูปที่ 4.1 The Memory Hierarchy**

กุญแจสำคัญของลำดับชั้นคือ **Locality of Reference** — ในช่วงเวลาสั้น โปรเซสเซอร์ส่วนใหญ่ทำงานกับกลุ่มการอ้างอิงหน่วยความจำที่คงที่

#### ตัวอย่าง 4.1

สมมติว่ามีหน่วยความจำสองระดับ:
- Level 1: 1,000 word, access time = 0.01 μs
- Level 2: 100,000 word, access time = 0.1 μs
- Hit ratio ของ Level 1 = 95%

เวลาเฉลี่ย:
$$(0.95)(0.01\ \mu s) + (0.05)(0.01 + 0.1\ \mu s) = 0.0095 + 0.0055 = 0.015\ \mu s$$

---

## 4.2 หลักการของหน่วยความจำแคช

หน่วยความจำแคชถูกออกแบบมาเพื่อรวมเวลาการเข้าถึงของหน่วยความจำที่เร็วและแพงเข้ากับขนาดของหน่วยความจำที่ถูกและช้ากว่า

**การทำงาน:**
1. โปรเซสเซอร์พยายามอ่าน word จากหน่วยความจำ
2. ตรวจสอบว่า word อยู่ใน cache หรือไม่
3. ถ้าใช่ (**cache hit**) → ส่ง word ให้โปรเซสเซอร์
4. ถ้าไม่ใช่ (**cache miss**) → ดึง block จาก main memory เข้า cache แล้วส่ง word ให้โปรเซสเซอร์

เนื่องจากหลักการ locality of reference เมื่อ block ถูกดึงเข้า cache แล้ว มีความเป็นไปได้สูงที่จะมีการอ้างอิงซ้ำในอนาคต

**โครงสร้าง Cache/Main Memory:**
- Main memory: $2^n$ words, แบ่งเป็น M = $2^n/K$ blocks (แต่ละ block มี K words)
- Cache: m lines (m << M), แต่ละ line มี K words + tag bits
- **Line size** = ความยาว line ไม่รวม tag และ control bits
- **Tag** = ระบุว่า block ใดกำลังอยู่ใน line

---

## 4.3 องค์ประกอบของการออกแบบแคช

**ตาราง 4.2 องค์ประกอบของการออกแบบแคช**

| Cache Addresses | Write Policy |
|---|---|
| Logical / Physical | Write through / Write back |
| **Cache Size** | **Line Size** |
| **Mapping Function** | **Number of Caches** |
| Direct / Associative / Set associative | Single/Two level, Unified/Split |
| **Replacement Algorithm** | |
| LRU, FIFO, LFU, Random | |

---

### ที่อยู่แคช

| ประเภท | คำอธิบาย | ข้อดี | ข้อเสีย |
|-------|---------|------|---------|
| **Logical (Virtual) Cache** | จัดเก็บโดยใช้ virtual address | เร็วกว่า เพราะไม่ต้องรอ MMU | ต้อง flush cache เมื่อสลับ application |
| **Physical Cache** | จัดเก็บโดยใช้ physical address | ไม่มีปัญหาเรื่อง aliasing | ช้ากว่าเล็กน้อย |

---

### ขนาดแคช

**ตาราง 4.3 ขนาดแคชของโปรเซสเซอร์บางตัว**

| Processor | ปี | L1 Cache | L2 Cache | L3 Cache |
|-----------|-----|----------|----------|----------|
| IBM 360/85 | 1968 | 16–32 kB | — | — |
| Intel 80486 | 1989 | 8 kB | — | — |
| Pentium 4 | 2000 | 8 kB/8 kB | 256 kB | — |
| Itanium 2 | 2002 | 32 kB | 256 kB | 6 MB |
| Intel Core i7 EE 990 | 2011 | 6×32 kB/32 kB | 1.5 MB | 12 MB |

*หมายเหตุ: ค่าสองตัวคั่นด้วย slash = instruction cache / data cache*

---

### ฟังก์ชันการแมป

#### ตัวอย่าง 4.2 (ใช้ตลอดหัวข้อนี้)

- Cache: 64 kB → 16K = 2¹⁴ lines ของ 4 bytes
- Main memory: 16 MB → 24-bit address, 4M blocks ของ 4 bytes

---

#### Direct Mapping

แมปแต่ละ block ของ main memory ไปยัง cache line เดียวที่แน่นอน:

$$i = j \bmod m$$

โดย $i$ = หมายเลข cache line, $j$ = หมายเลข main memory block, $m$ = จำนวน line

**โครงสร้างที่อยู่ (ตัวอย่าง 24-bit):**

| Tag (8 bits) | Line (14 bits) | Word (2 bits) |
|:---:|:---:|:---:|
| ระบุว่า block ใด | ระบุ cache line | ระบุ byte ใน line |

**ข้อดี:** ง่ายและราคาถูกในการนำไปใช้  
**ข้อเสีย:** อาจเกิด **thrashing** ถ้าโปรแกรมอ้างอิง block ต่างสองตัวที่แมปไปยัง line เดียวกันซ้ำๆ

---

#### Associative Mapping

อนุญาตให้ block ใดก็ได้โหลดเข้า line ใดก็ได้:

**โครงสร้างที่อยู่:**

| Tag (22 bits) | Word (2 bits) |
|:---:|:---:|
| ระบุ block โดยตรง | ระบุ byte ใน line |

**ข้อดี:** ยืดหยุ่นสูงสุด ลด thrashing  
**ข้อเสีย:** ต้องตรวจสอบ tag ทุก line พร้อมกัน → วงจรซับซ้อน

---

#### Set-Associative Mapping

การประนีประนอมระหว่าง Direct และ Associative:

$$m = v \times k \qquad i = j \bmod v$$

โดย $v$ = จำนวน set, $k$ = จำนวน line ต่อ set (**k-way set-associative**)

**โครงสร้างที่อยู่ (2-way, ตัวอย่าง):**

| Tag (9 bits) | Set (13 bits) | Word (2 bits) |
|:---:|:---:|:---:|
| ระบุว่า block ใด | ระบุ set | ระบุ byte |

| กรณีพิเศษ | ผล |
|----------|-----|
| v = m, k = 1 | = Direct mapping |
| v = 1, k = m | = Associative mapping |

---

### อัลกอริทึมการแทนที่

| อัลกอริทึม | คำอธิบาย | ข้อดี |
|----------|---------|------|
| **LRU (Least Recently Used)** | แทนที่ block ที่ไม่ได้ใช้นานที่สุด | Hit ratio ดีที่สุด |
| **FIFO (First In First Out)** | แทนที่ block ที่อยู่ใน cache นานที่สุด | ง่ายต่อการนำไปใช้ |
| **LFU (Least Frequently Used)** | แทนที่ block ที่ถูกอ้างอิงน้อยที่สุด | ตอบสนองความถี่ |
| **Random** | เลือก block แบบสุ่ม | ง่ายมาก |

---

### นโยบายการเขียน

| นโยบาย | วิธีการ | ข้อดี | ข้อเสีย |
|--------|---------|------|---------|
| **Write Through** | เขียนทั้ง cache และ main memory พร้อมกัน | Main memory ถูกต้องเสมอ | สร้าง memory traffic มาก |
| **Write Back** | เขียนเฉพาะ cache; เขียน memory เมื่อ block ถูกแทนที่ | ลดการเขียน memory | Main memory อาจ invalid |

**Dirty bit:** บิตที่บอกว่า cache line ถูกแก้ไขแล้วหรือไม่ (ใช้ใน Write Back)

#### Cache Coherency

ปัญหาเมื่อหลายโปรเซสเซอร์ใช้ cache แยกกัน แนวทางแก้ไข:
- **Bus watching with write through:** แต่ละ cache controller ตรวจสอบ write operation บน bus
- **Hardware transparency:** ใช้ hardware พิเศษ synchronize cache ทั้งหมด
- **Noncacheable memory:** กำหนด shared memory เป็น noncacheable

---

### ขนาดบรรทัด

เมื่อขนาด block เพิ่มขึ้น:
- ขนาดเล็ก → hit ratio เพิ่มขึ้น (เพราะ locality)
- ขนาดใหญ่เกินไป → hit ratio ลดลง เพราะ:
  1. Block ใหญ่ → ลดจำนวน line ใน cache
  2. ข้อมูลที่ดึงมาอาจไม่ได้ใช้ทั้งหมด

**ขนาดที่เหมาะสม:** โดยทั่วไป 4–8 addressable units

---

### จำนวนแคช

**Single vs. Two Level Cache:**

| | ข้อดี |
|-|------|
| **One-level** | ง่าย |
| **Two-level (L1, L2)** | L1 เร็วมาก, L2 ใหญ่กว่า ชดเชยการ miss ของ L1 |

**Unified vs. Split Cache:**

| | คำอธิบาย |
|-|---------|
| **Unified** | Instruction และ Data ใช้ cache เดียวกัน |
| **Split** | แยก Instruction cache และ Data cache → ลด contention ใน pipeline |

---

## 4.4 การจัดการแคชของ Pentium 4

Pentium 4 มี cache organization ดังนี้:

- **L1 Instruction Cache:** 12K micro-ops trace cache
- **L1 Data Cache:** 8 kB, 4-way set associative, line size 64 bytes
- **L2 Cache:** 256 kB, 8-way set associative, unified, line size 128 bytes
- **L3 Cache:** (รุ่นบางรุ่น) 1–8 MB

**Write Policy:** Write back สำหรับทั้ง L1 และ L2

---

## ภาคผนวก 4A

## คุณลักษณะด้านประสิทธิภาพของหน่วยความจำสองระดับ

### Locality

**Locality of Reference** มีสองรูปแบบ:
- **Temporal locality:** ตำแหน่งที่เพิ่งถูกอ้างอิงมีแนวโน้มจะถูกอ้างอิงอีกในเร็วๆ นี้
- **Spatial locality:** ตำแหน่งใกล้เคียงกับตำแหน่งที่เพิ่งถูกอ้างอิงมีแนวโน้มจะถูกอ้างอิงในเร็วๆ นี้

### ประสิทธิภาพ

สูตรเวลาเฉลี่ยในการเข้าถึง:

$$T_{avg} = H \cdot T_1 + (1-H)(T_1 + T_2)$$

หรือ:

$$T_{avg} = T_1 + (1-H) \cdot T_2$$

โดย $H$ = hit ratio (สัดส่วนที่พบใน Level 1), $T_1$ = access time ของ Level 1, $T_2$ = access time ของ Level 2

**สรุป:** เพื่อให้ได้ประสิทธิภาพดี ต้องการ H ใกล้เคียง 1 ซึ่งเป็นไปได้เพราะ locality of reference

---

*จัดทำสรุปเพื่อใช้ประกอบการเรียนวิชา Computer Organization and Architecture*
