# บทที่ 8: การสนับสนุนของระบบปฏิบัติการ (Operating System Support)

## 📌 ภาพรวมของบท

บทนี้อธิบายบทบาทของระบบปฏิบัติการ (OS) ในการควบคุมทรัพยากรฮาร์ดแวร์ โดยเน้น 2 หัวข้อหลัก คือ **การจัดตารางเวลา (Scheduling)** และ **การจัดการหน่วยความจำ (Memory Management)**

---

## 8.1 ภาพรวมระบบปฏิบัติการ (Operating System Overview)

### 🎯 วัตถุประสงค์ของ OS
ระบบปฏิบัติการมีวัตถุประสงค์หลัก 2 ประการ:

| วัตถุประสงค์ | คำอธิบาย |
|---|---|
| **ความสะดวก (Convenience)** | ทำให้คอมพิวเตอร์ใช้งานได้ง่ายขึ้น |
| **ประสิทธิภาพ (Efficiency)** | ทำให้ใช้ทรัพยากรระบบได้อย่างคุ้มค่า |

### 🔧 บริการที่ OS ให้

1. **การสร้างโปรแกรม (Program Creation)** — จัดเตรียมเครื่องมือ เช่น editor และ debugger
2. **การรันโปรแกรม (Program Execution)** — โหลดคำสั่งและข้อมูลลงหน่วยความจำ และเตรียมทรัพยากรที่จำเป็น
3. **การเข้าถึง I/O (Access to I/O Devices)** — จัดการรายละเอียดของอุปกรณ์ I/O แทนโปรแกรมเมอร์
4. **การเข้าถึงไฟล์ (Controlled Access to Files)** — ควบคุมการอ่าน/เขียนไฟล์และป้องกันการเข้าถึงโดยไม่ได้รับอนุญาต
5. **การเข้าถึงระบบ (System Access)** — ป้องกันและจัดการทรัพยากรสำหรับผู้ใช้หลายคน
6. **การตรวจจับและตอบสนองต่อข้อผิดพลาด (Error Detection and Response)** — จัดการข้อผิดพลาดทั้งฮาร์ดแวร์และซอฟต์แวร์
7. **การบัญชี (Accounting)** — เก็บสถิติการใช้ทรัพยากรและตรวจสอบประสิทธิภาพ

### 🏗️ อินเทอร์เฟซหลักในระบบคอมพิวเตอร์

| อินเทอร์เฟซ | ความหมาย |
|---|---|
| **ISA (Instruction Set Architecture)** | ขอบเขตระหว่างฮาร์ดแวร์และซอฟต์แวร์ กำหนดชุดคำสั่งเครื่อง |
| **ABI (Application Binary Interface)** | กำหนดมาตรฐานความสามารถพกพาของ binary และอินเทอร์เฟซ system call |
| **API (Application Programming Interface)** | ให้โปรแกรมเมอร์เข้าถึงทรัพยากรผ่านภาษาระดับสูง ทำให้ port โปรแกรมได้ง่าย |

### 💡 OS ในฐานะตัวจัดการทรัพยากร
- OS เป็นโปรแกรมที่รันบนโปรเซสเซอร์เหมือนโปรแกรมทั่วไป
- OS ต้องสละการควบคุม (relinquish control) ให้โปรแกรมอื่นทำงาน แล้วค่อยกลับมาควบคุมอีกครั้ง
- **Kernel (นิวเคลียส)** คือส่วนของ OS ที่อยู่ในหน่วยความจำหลักตลอดเวลา

---

### 📜 ประเภทของ OS

#### ระบบยุคแรก (Early Systems)
- ไม่มี OS โปรแกรมเมอร์ควบคุมฮาร์ดแวร์โดยตรง
- ปัญหาหลัก: การจัดตารางเวลา (ใช้ sign-up sheet) และเวลาเตรียมระบบ (setup time) ที่สูญเสียไป

#### Simple Batch Systems
- ใช้ **Monitor** — โปรแกรมที่ควบคุมลำดับการทำงาน
- **Resident Monitor** — ส่วนของ Monitor ที่อยู่ในหน่วยความจำตลอดเวลา
- ใช้ **JCL (Job Control Language)** เพื่อสั่งงาน Monitor
- ต้องการฮาร์ดแวร์สนับสนุน: memory protection, timer, privileged instructions, interrupts

#### Multiprogrammed Batch Systems
- เก็บหลายโปรแกรมในหน่วยความจำพร้อมกัน
- เมื่อโปรแกรมหนึ่งรอ I/O โปรเซสเซอร์สลับไปทำโปรแกรมอื่น
- เพิ่มประสิทธิภาพการใช้ CPU อย่างมีนัยสำคัญ

**ตัวอย่างเปรียบเทียบ (Example 8.1):**

| ตัวชี้วัด | Uniprogramming | Multiprogramming |
|---|---|---|
| การใช้ CPU | 20% | 40% |
| การใช้หน่วยความจำ | 33% | 67% |
| เวลารวม (นาที) | 30 | 15 |
| throughput (งาน/ชั่วโมง) | 6 | 12 |
| เวลาตอบสนองเฉลี่ย (นาที) | 18 | 10 |

#### Time-Sharing Systems
- โปรเซสเซอร์สลับระหว่างผู้ใช้หลายคนในช่วงเวลาสั้น ๆ (quantum)
- ผู้ใช้แต่ละคนได้รับ 1/n ของความเร็วโปรเซสเซอร์ (n = จำนวนผู้ใช้)

**ความแตกต่าง Batch vs Time-Sharing:**

| | Batch Multiprogramming | Time Sharing |
|---|---|---|
| เป้าหมายหลัก | เพิ่มการใช้งาน CPU | ลดเวลาตอบสนอง |
| คำสั่งมาจาก | JCL | คำสั่งที่พิมพ์ที่ terminal |

---

## 8.2 การจัดตารางเวลา (Scheduling)

### 🔄 แนวคิด Process
**Process** คือโปรแกรมที่กำลังทำงาน — นิยามได้หลายแบบ:
- โปรแกรมที่กำลัง execute
- สิ่งที่ processor กำลังประมวลผล

### 📊 ประเภทของ Scheduling

| ประเภท | หน้าที่ |
|---|---|
| **Long-Term Scheduling** | ตัดสินใจว่าโปรแกรมใดจะเข้าสู่ระบบ (ควบคุมระดับ multiprogramming) |
| **Medium-Term Scheduling** | ตัดสินใจว่า process ใดจะถูก swap เข้า/ออกหน่วยความจำ |
| **Short-Term Scheduling** | ตัดสินใจว่า process ใดจะได้ใช้ CPU ต่อไป (dispatcher) |
| **I/O Scheduling** | ตัดสินใจว่า I/O request ใดจะได้รับบริการ |

### 🔁 Long-Term Scheduling
- ควบคุมจำนวน process ในหน่วยความจำ
- เกณฑ์การตัดสินใจ: priority, เวลาประมวลผลที่คาดไว้, ความต้องการ I/O
- ในระบบ time-sharing: รับผู้ใช้ทุกคนจนระบบเต็ม

### 🔁 Medium-Term Scheduling
- เป็นส่วนหนึ่งของ swapping
- ตัดสินใจตาม: ระดับ multiprogramming และความต้องการหน่วยความจำ

### 🔁 Short-Term Scheduling (Dispatcher)
- ทำงานบ่อยที่สุด ตัดสินใจละเอียดที่สุด
- ใช้ **round-robin algorithm** เป็นพื้นฐาน

### 📋 สถานะของ Process (5 States)

```
[New] → Admit → [Ready] → Dispatch → [Running] → Release → [Exit]
                    ↑          ↓ Timeout        ↓
                    |      Event occurs    Event wait
                    └──────────[Blocked]←──────┘
```

| สถานะ | ความหมาย |
|---|---|
| **New** | เพิ่งได้รับการยอมรับ กำลังถูก initialize |
| **Ready** | พร้อมทำงาน รอโปรเซสเซอร์ |
| **Running** | กำลังถูก execute โดยโปรเซสเซอร์ |
| **Waiting/Blocked** | หยุดรอทรัพยากร เช่น I/O |
| **Halted/Exit** | สิ้นสุดการทำงานแล้ว |

### 📦 Process Control Block (PCB)
ข้อมูลที่ OS เก็บสำหรับแต่ละ process:
- **Identifier** — รหัสเฉพาะของ process
- **State** — สถานะปัจจุบัน
- **Priority** — ระดับความสำคัญ
- **Program Counter** — ตำแหน่งคำสั่งถัดไป
- **Memory Pointers** — ตำแหน่งเริ่มต้น/สิ้นสุดในหน่วยความจำ
- **Context Data** — ข้อมูลในรีจิสเตอร์ขณะทำงาน
- **I/O Status Information** — คำร้อง I/O ที่ค้างอยู่
- **Accounting Information** — เวลา CPU ที่ใช้ไป

### 🗂️ Queue Management
- **Long-Term Queue** — รายการงานที่รอเข้าระบบ
- **Short-Term Queue** — process ทั้งหมดในสถานะ Ready
- **I/O Queues** — process ที่รออุปกรณ์ I/O แต่ละตัว

---

## 8.3 การจัดการหน่วยความจำ (Memory Management)

### 💾 Swapping
- เมื่อทุก process กำลังรอ I/O โปรเซสเซอร์จะว่างเปล่า
- แนวทางแก้ไข: **Swap** process ออกไปดิสก์ชั่วคราว แล้วนำ process อื่นเข้ามา
- มี **Intermediate Queue** สำหรับ process ที่ถูก swap ออก
- Swapping เป็น I/O operation แต่ disk I/O เร็วกว่า I/O อื่น ๆ จึงยังช่วยเพิ่มประสิทธิภาพได้

### 🔲 Partitioning (การแบ่งส่วน)

#### Fixed-Size Partitions
- แบ่งหน่วยความจำเป็นส่วนขนาดคงที่ (ไม่จำเป็นต้องเท่ากัน)
- ปัญหา: มีพื้นที่เสียหาย (wasted memory) เพราะ process ไม่ได้ใช้เต็ม partition

#### Variable-Size Partitions
- จัดสรรพื้นที่ตามความต้องการจริงของแต่ละ process
- ปัญหา: **Fragmentation** — เกิด "รู" (holes) ขนาดเล็กกระจัดกระจาย
- แนวทางแก้ไข: **Compaction** — เลื่อน process ในหน่วยความจำให้พื้นที่ว่างรวมอยู่ด้วยกัน (แต่สิ้นเปลือง CPU)

#### Logical vs Physical Address
- **Logical Address** — ตำแหน่งสัมพัทธ์จากต้นโปรแกรม (ใช้ใน program)
- **Physical Address** — ตำแหน่งจริงในหน่วยความจำ
- โปรเซสเซอร์แปลง logical → physical โดยบวก **base address**

### 📄 Paging

**แนวคิดหลัก:**
- แบ่งหน่วยความจำเป็น **frames** ขนาดเท่ากัน
- แบ่งโปรแกรมเป็น **pages** ขนาดเท่ากับ frame
- page ของโปรแกรมสามารถอยู่ใน frame ใดก็ได้ (ไม่ต้องติดกัน)

**Page Table:**
- OS เก็บ page table สำหรับแต่ละ process
- ทำหน้าที่แมป: page number → frame number
- **Logical Address** = (page number, offset)
- **Physical Address** = (frame number, offset)

**ข้อดีของ Paging:**
- ไม่มีปัญหา fragmentation ภายนอก
- สูญเสียพื้นที่เพียงเศษของ page สุดท้าย

### 🌐 Virtual Memory

#### Demand Paging
- โหลด page เข้าหน่วยความจำเฉพาะเมื่อต้องการ (**on demand**)
- อาศัย **Principle of Locality** — โปรแกรมมักอ้างอิงหน่วยความจำในบริเวณเดิม
- เมื่อต้องการ page ที่ไม่อยู่ใน RAM: เกิด **Page Fault** → OS โหลด page จากดิสก์

**Page Replacement:**
- เมื่อหน่วยความจำเต็ม ต้องเลือก page ที่จะ swap ออก
- หากเลือกผิด page ที่ถูก swap ออกอาจถูกต้องการในทันที → **Thrashing**
- อัลกอริทึมที่ใช้: **LRU (Least Recently Used)** เป็นพื้นฐาน

**ผลสำคัญของ Virtual Memory:**
- Process สามารถมีขนาดใหญ่กว่าหน่วยความจำจริงได้
- **Real Memory** = หน่วยความจำจริง (RAM)
- **Virtual Memory** = พื้นที่บนดิสก์ที่โปรแกรมมองเห็นว่าเป็นหน่วยความจำ

#### Page Table Structures

| โครงสร้าง | คำอธิบาย |
|---|---|
| Single-level | 1 page table ต่อ process |
| Two-level | มี page directory ที่ชี้ไป page tables |
| Inverted Page Table | 1 entry ต่อ frame จริง (ใช้ใน PowerPC, UltraSPARC) ใช้ hashing |

### ⚡ Translation Lookaside Buffer (TLB)
- Cache พิเศษสำหรับ page table entries
- ปัญหา: การแปลงทุก virtual address ต้องเข้าถึงหน่วยความจำ 2 ครั้ง (page table + ข้อมูล)
- TLB เก็บ page table entries ที่ใช้ล่าสุด
- ด้วย Principle of Locality: ส่วนใหญ่จะ **TLB Hit** → ประหยัดเวลา

**ลำดับการทำงาน:**
```
Virtual Address → ตรวจสอบ TLB
  ├── TLB Hit → ได้ Physical Address ทันที
  └── TLB Miss → ค้นหาใน Page Table
                  ├── Page อยู่ใน RAM → อัพเดท TLB
                  └── Page Fault → โหลดจาก Disk → อัพเดท Page Table → อัพเดท TLB
```

### 🗂️ Segmentation (การแบ่งเป็นส่วน)
- ผู้ใช้มองหน่วยความจำเป็น **segments** หลายชิ้น (แต่ละชิ้นมีขนาดต่างกัน)
- **ต่างจาก Paging**: segmentation มองเห็นโดยโปรแกรมเมอร์, paging โปรแกรมเมอร์ไม่เห็น
- Address format: (segment number, offset)

**ข้อดีของ Segmentation:**
1. ขยาย/ลด data structure ได้ยืดหยุ่น
2. แก้ไขโปรแกรมและ compile ใหม่ได้โดยไม่กระทบส่วนอื่น
3. แชร์ segment ระหว่าง process ได้
4. กำหนดสิทธิ์การเข้าถึง (protection) ได้ละเอียด

---

## 8.4 การจัดการหน่วยความจำของ Intel x86

### มุมมองหน่วยความจำ 4 แบบ

| โหมด | คำอธิบาย |
|---|---|
| Unsegmented Unpaged | Virtual address = Physical address |
| Unsegmented Paged | ใช้ paging อย่างเดียว (เช่น Berkeley UNIX) |
| Segmented Unpaged | ใช้ segmentation อย่างเดียว |
| Segmented Paged | ใช้ทั้ง segmentation และ paging (เช่น UNIX System V) |

### Segmentation ใน x86
- Virtual address = 16-bit segment selector + 32-bit offset
- มี 4 ระดับสิทธิ์ (Privilege Levels 0–3): ระดับ 0 = สูงสุด (OS core), ระดับ 3 = ต่ำสุด (application)
- Virtual memory space: 2⁴⁶ = 64 TB

### Paging ใน x86
- Two-level table lookup: **Page Directory** → **Page Table** → **Page**
- Page Directory: สูงสุด 1,024 entries
- Page Table: สูงสุด 1,024 entries (4 KB ต่อ page)
- รองรับ 2 ขนาด page: 4 KB และ 4 MB (ถ้าเปิด PSE)
- TLB เก็บ page table entries ได้ 32 entries

---

## 8.5 การจัดการหน่วยความจำของ ARM

### Memory System Organization
- ใช้ TLB เป็น cache ของ page table entries
- ถ้า TLB hit → ส่ง physical address ไปยัง main memory โดยตรง
- มี Access Control Hardware ตรวจสอบสิทธิ์การเข้าถึง

### ขนาดของหน่วยความจำที่รองรับ

| ประเภท | ขนาด |
|---|---|
| Supersections | 16 MB |
| Sections | 1 MB |
| Large Pages | 64 KB |
| Small Pages | 4 KB |

### Two-Level Address Translation (Small Pages)
- **L1 Table**: 4,096 entries (index จาก 12 bits สูงสุด)
- **L2 Table**: 256 entries (index จาก 8 bits ถัดมา)
- **Page Index**: 12 bits ต่ำสุด
- L1 entry มี 4 รูปแบบ: Fault, Page Table, Section, Supersection

### Access Control และ Domains
- **AP bits** — ควบคุมสิทธิ์: no access, read-only, read-write
- **Domain** — กลุ่มของ sections/pages ที่มีสิทธิ์การเข้าถึงร่วมกัน (รองรับ 16 domains)
- **Clients** — ผู้ใช้ domain ที่ต้องปฏิบัติตาม access permissions
- **Managers** — ควบคุม domain และข้ามผ่าน access permissions ได้

---

## 📊 ตารางเปรียบเทียบ: Paging vs Segmentation

| ประเด็น | Paging | Segmentation |
|---|---|---|
| การมองเห็นของโปรแกรมเมอร์ | ไม่เห็น (transparent) | เห็นได้ |
| ขนาดของหน่วย | คงที่ | แปรผัน |
| การจัดการ fragmentation | ดีกว่า | มี external fragmentation |
| การ protect และ share | ทำได้ยาก | ทำได้ง่าย |
| จุดประสงค์หลัก | ขยายพื้นที่หน่วยความจำ | จัดระเบียบโปรแกรม |

---

## 🗝️ Key Terms สำคัญ

| คำศัพท์ | ความหมาย |
|---|---|
| **Batch System** | ระบบที่ประมวลผลงานเป็นชุด |
| **Demand Paging** | โหลด page เมื่อต้องการเท่านั้น |
| **Kernel** | แกนหลักของ OS ที่อยู่ใน RAM ตลอดเวลา |
| **Logical Address** | ตำแหน่งสัมพัทธ์ในโปรแกรม |
| **Physical Address** | ตำแหน่งจริงใน RAM |
| **Multiprogramming** | รันหลายโปรแกรมพร้อมกัน |
| **Page Fault** | เมื่อ page ที่ต้องการไม่อยู่ใน RAM |
| **Process** | โปรแกรมที่กำลัง execute |
| **Process Control Block** | โครงสร้างข้อมูลของ process ใน OS |
| **Real Memory** | หน่วยความจำจริง (RAM) |
| **Segmentation** | การแบ่งหน่วยความจำเป็น segments |
| **Swapping** | การย้าย process ระหว่าง RAM และดิสก์ |
| **Thrashing** | CPU เสียเวลา swap pages มากกว่า execute |
| **TLB** | Cache สำหรับ page table entries |
| **Virtual Memory** | พื้นที่หน่วยความจำสมมติที่ใช้ดิสก์ |

---

---

# 🧪 แบบทดสอบท้ายบท (Quiz)

## ส่วนที่ 1: คำถามปรนัย (Multiple Choice)

**ข้อ 1.** ข้อใดคือวัตถุประสงค์หลักของระบบปฏิบัติการ (OS)?

- A) เพิ่มความเร็วของ CPU
- B) ความสะดวกในการใช้งานและประสิทธิภาพในการใช้ทรัพยากร ✅
- C) ลดต้นทุนฮาร์ดแวร์
- D) เพิ่มความจุของดิสก์

---

**ข้อ 2.** ส่วนใดของ OS ที่อยู่ในหน่วยความจำหลักตลอดเวลา?

- A) Utility Programs
- B) Device Drivers
- C) Kernel (Nucleus) ✅
- D) Compiler

---

**ข้อ 3.** Multiprogramming แตกต่างจาก Uniprogramming อย่างไร?

- A) Multiprogramming ใช้ CPU เร็วกว่า
- B) Multiprogramming รันหลายโปรแกรมพร้อมกันเพื่อเพิ่มการใช้งาน CPU ✅
- C) Uniprogramming ใช้หน่วยความจำมากกว่า
- D) Multiprogramming ต้องใช้ CPU หลายตัว

---

**ข้อ 4.** Long-Term Scheduler มีหน้าที่ใด?

- A) ตัดสินใจว่า process ใดจะได้ใช้ CPU ต่อไป
- B) ควบคุมการ swap process เข้า/ออก RAM
- C) กำหนดว่าโปรแกรมใดจะได้เข้าสู่ระบบและควบคุมระดับ multiprogramming ✅
- D) จัดการคิว I/O

---

**ข้อ 5.** Short-Term Scheduler หรือ Dispatcher ทำหน้าที่ใด?

- A) เลือกว่างานใดจะเข้าสู่ระบบ
- B) ตัดสินใจว่า process ใดจะได้รับ CPU ต่อไป ✅
- C) จัดการ virtual memory
- D) ควบคุมการ swapping

---

**ข้อ 6.** Process ที่กำลังรอ I/O จะอยู่ในสถานะใด?

- A) Ready
- B) Running
- C) New
- D) Waiting (Blocked) ✅

---

**ข้อ 7.** ข้อใดอธิบาย "Thrashing" ได้ถูกต้องที่สุด?

- A) หน่วยความจำขาดแคลน
- B) CPU ใช้เวลาส่วนใหญ่ในการ swap pages แทนการ execute โปรแกรม ✅
- C) process ทำงานซ้ำกัน
- D) การสูญเสียข้อมูลในหน่วยความจำ

---

**ข้อ 8.** TLB (Translation Lookaside Buffer) ถูกสร้างขึ้นเพื่อแก้ปัญหาใด?

- A) หน่วยความจำไม่เพียงพอ
- B) การเข้าถึง page table ต้องใช้เวลา 2 ครั้ง ทำให้ช้า ✅
- C) fragmentation ของหน่วยความจำ
- D) การจัดการ I/O ที่ซับซ้อน

---

**ข้อ 9.** ข้อใด**ไม่ใช่**ข้อดีของ Segmentation เมื่อเทียบกับ Paging?

- A) ขยาย data structure ได้ง่ายกว่า
- B) แชร์ segment ระหว่าง process ได้
- C) ไม่มี external fragmentation ✅
- D) กำหนดสิทธิ์การเข้าถึงได้ละเอียดกว่า

---

**ข้อ 10.** Intel x86 รองรับกี่ระดับสิทธิ์ (Privilege Levels)?

- A) 2
- B) 3
- C) 4 ✅
- D) 8

---

**ข้อ 11.** "Demand Paging" หมายความว่าอย่างไร?

- A) โหลดทุก page ของโปรแกรมก่อนเริ่มทำงาน
- B) โหลด page เข้าหน่วยความจำเฉพาะเมื่อต้องการใช้งาน ✅
- C) จัดเก็บ page ตามลำดับการใช้งาน
- D) ลบ page ที่ไม่ได้ใช้ออกจากหน่วยความจำ

---

**ข้อ 12.** ในระบบ ARM memory management, "Supersection" มีขนาดเท่าใด?

- A) 4 KB
- B) 64 KB
- C) 1 MB
- D) 16 MB ✅

---

**ข้อ 13.** Process Control Block (PCB) ประกอบด้วยข้อมูลใดบ้าง?

- A) เฉพาะ Program Counter และ Memory Pointers
- B) identifier, state, priority, program counter, memory pointers, context data, I/O status, accounting info ✅
- C) เฉพาะข้อมูลที่เกี่ยวกับ I/O
- D) เฉพาะ context data และ priority

---

**ข้อ 14.** การแปลง Logical Address เป็น Physical Address ใน paging ทำโดย?

- A) OS เท่านั้น
- B) โปรแกรมเมอร์กำหนดเอง
- C) Hardware ของโปรเซสเซอร์โดยใช้ Page Table ✅
- D) Disk Controller

---

**ข้อ 15.** ใน Intel x86 paging, Page Directory มีสูงสุดกี่ entries?

- A) 256
- B) 512
- C) 1,024 ✅
- D) 4,096

---

## ส่วนที่ 2: คำถามอัตนัยสั้น (Short Answer)

**ข้อ 16.** อธิบายความแตกต่างระหว่าง "Real Memory" และ "Virtual Memory"

> **เฉลย:** Real Memory คือหน่วยความจำจริง (RAM) ที่โปรแกรมรันอยู่ ในขณะที่ Virtual Memory คือพื้นที่หน่วยความจำสมมติที่ขยายออกไปยังดิสก์ โดยโปรแกรมเมอร์มองเห็นว่ามีหน่วยความจำมาก (ขนาดใกล้เคียงดิสก์) แม้ RAM จะมีน้อย ทำให้ process มีขนาดใหญ่กว่า RAM ทั้งหมดได้

---

**ข้อ 17.** อธิบายว่า Swapping ช่วยแก้ปัญหาอะไร และมีข้อเสียอย่างไร?

> **เฉลย:** Swapping แก้ปัญหา CPU ว่างเปล่าเมื่อทุก process ใน RAM กำลังรอ I/O โดยย้าย process ที่ไม่พร้อมทำงานออกไปดิสก์แล้วนำ process ใหม่เข้ามา ข้อเสียคือ Swapping ก็เป็น I/O operation เช่นกัน ซึ่งอาจทำให้ระบบช้าลง อย่างไรก็ตาม disk I/O เร็วกว่า I/O อื่น ๆ ดังนั้นโดยทั่วไปจึงยังเพิ่มประสิทธิภาพได้

---

**ข้อ 18.** อธิบายความแตกต่างของ Fixed-Size Partitions และ Variable-Size Partitions และปัญหาของแต่ละแบบ

> **เฉลย:** Fixed-Size Partitions แบ่งหน่วยความจำเป็นส่วนคงที่ ปัญหาคือพื้นที่เสียหาย (Internal Fragmentation) เพราะ process ไม่ได้ใช้เต็ม partition. Variable-Size Partitions จัดสรรตามความต้องการจริง แต่เกิด External Fragmentation ทำให้มีรูเล็ก ๆ กระจัดกระจายที่ไม่สามารถใช้งานได้ แก้ไขด้วย Compaction แต่สิ้นเปลือง CPU

---

**ข้อ 19.** อธิบายการทำงานของ TLB ในการแปลง Virtual Address เป็น Physical Address

> **เฉลย:** เมื่อ CPU สร้าง virtual address, ระบบค้นหา page table entry ใน TLB ก่อน ถ้า TLB Hit → นำ frame number รวมกับ offset เพื่อสร้าง physical address ทันที ถ้า TLB Miss → เข้าถึง page table ใน RAM หาก page อยู่ใน RAM ก็อัพเดท TLB และสร้าง physical address หากเกิด Page Fault ก็โหลด page จากดิสก์มาก่อน

---

**ข้อ 20.** อธิบายว่า "Principle of Locality" เกี่ยวข้องกับ Virtual Memory และ TLB อย่างไร?

> **เฉลย:** Principle of Locality บอกว่าโปรแกรมมักอ้างอิงหน่วยความจำในบริเวณเดิมซ้ำ ๆ ในช่วงเวลาสั้น ๆ ดังนั้น Virtual Memory จึงโหลดแค่ไม่กี่ pages ที่ต้องการจริง (Demand Paging) แทนที่จะโหลดทั้งหมด และ TLB ก็ได้ประโยชน์เพราะ page table entries ที่ใช้บ่อยจะอยู่ใน TLB ทำให้ TLB Hit rate สูง ลดการเข้าถึง RAM เพื่อค้นหา page table

---

## ส่วนที่ 3: คำถามวิเคราะห์ (Analysis)

**ข้อ 21.** จากตัวอย่าง Example 8.1 ในบทเรียน: ในระบบ Multiprogramming ทำไม throughput จึงเพิ่มจาก 6 เป็น 12 งาน/ชั่วโมง และเวลารวมลดจาก 30 เป็น 15 นาที?

> **เฉลย:** เนื่องจาก JOB1 เน้น CPU, JOB2 เน้น terminal, JOB3 เน้น disk/printer ทั้งสามงานใช้ทรัพยากรคนละประเภท จึงสามารถทำงานพร้อมกันโดยแทบไม่มีการแย่งทรัพยากรกัน ใน Multiprogramming CPU ไม่ต้องรอ I/O ว่าง แต่สลับไปทำงานอื่นได้ ทำให้ทรัพยากรทุกชนิดถูกใช้งานพร้อมกัน ส่งผลให้เวลารวมลดลงครึ่งหนึ่ง

---

**ข้อ 22.** เปรียบเทียบข้อดีและข้อเสียของ Paging และ Segmentation ในด้านการจัดการหน่วยความจำ

> **เฉลย:**  
> **Paging** — ข้อดี: ไม่มี external fragmentation, จัดการง่าย, โปรแกรมเมอร์ไม่ต้องรับรู้. ข้อเสีย: อาจมี internal fragmentation (page สุดท้าย), ยากต่อการ protect/share ข้อมูล  
> **Segmentation** — ข้อดี: protect และ share ข้อมูลได้ดี, ขยาย data structure ได้ยืดหยุ่น, โปรแกรมเมอร์ควบคุมได้. ข้อเสีย: เกิด external fragmentation, ต้องทำ compaction

---

**ข้อ 23.** ทำไม Intel x86 จึงออกแบบให้รองรับทั้ง Segmentation และ Paging? และระบบปฏิบัติการอย่าง UNIX ใช้ประโยชน์จากการรวมกันนี้อย่างไร?

> **เฉลย:** การรวม Segmentation + Paging ให้ประโยชน์ของทั้งสอง: Segmentation ใช้จัดระเบียบโปรแกรมและกำหนด access control ในระดับ logical ในขณะที่ Paging จัดการ physical memory อย่างมีประสิทธิภาพ UNIX System V ใช้ Segmentation เพื่อแยก code segment จาก data segment พร้อม access rights ที่เหมาะสม และใช้ Paging เพื่อจัดการการใช้หน่วยความจำจริงโดยไม่ต้องโหลดทั้ง segment พร้อมกัน

---

## 📋 เฉลยรวม (Answer Key)

| ข้อ | เฉลย | ข้อ | เฉลย |
|---|---|---|---|
| 1 | B | 9 | C |
| 2 | C | 10 | C |
| 3 | B | 11 | B |
| 4 | C | 12 | D |
| 5 | B | 13 | B |
| 6 | D | 14 | C |
| 7 | B | 15 | C |
| 8 | B | 16-23 | อัตนัย |

---

*สรุปจาก: Computer Organization and Architecture, Chapter 8 — Operating System Support*
