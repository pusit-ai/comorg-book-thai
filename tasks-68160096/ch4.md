# 📘 Cache Memory (Chapter 4)

สรุปเนื้อหา **Computer Organization – Chapter 4: Cache Memory**
ครอบคลุมหัวข้อ **4.1 – 4.5**

---

# 📑 Table of Contents

* [4.1 Computer Memory System Overview](#41-computer-memory-system-overview)
* [4.2 Cache Memory Principles](#42-cache-memory-principles)
* [4.3 Elements of Cache Design](#43-elements-of-cache-design)
* [4.4 Pentium 4 Cache Organization](#44-pentium-4-cache-organization)
* [4.5 Key Terms](#45-key-terms)

---

# 4.1 Computer Memory System Overview

Computer memory มีหลายประเภททั้งในด้าน **technology, organization, performance และ cost**
ดังนั้นระบบคอมพิวเตอร์จึงใช้ **Memory Hierarchy** เพื่อให้ได้ทั้งความเร็วและความจุที่เหมาะสม

---

## Characteristics of Memory Systems

### Location

* **Internal Memory**

  * Registers
  * Cache
  * Main Memory

* **External Memory**

  * Magnetic Disk
  * Optical Disk
  * Magnetic Tape

---

### Capacity

* Number of **Words**
* Number of **Bytes**

---

### Unit of Transfer

* **Word**
* **Block**

---

### Access Method

* **Sequential Access** – เช่น Tape
* **Direct Access** – เช่น Disk
* **Random Access** – เช่น RAM
* **Associative Access** – ค้นหาจาก content

---

### Performance Metrics

* **Access Time (Latency)**
* **Memory Cycle Time**
* **Transfer Rate**

```
Tn = TA + (n / R)
```

Where

* `Tn` = Average time to read n bits
* `TA` = Access time
* `R` = Transfer rate

---

## Memory Hierarchy

```
Registers
   ↓
Cache
   ↓
Main Memory
   ↓
Magnetic Disk
   ↓
Optical Storage
   ↓
Magnetic Tape
```

| Property            | Trend     |
| ------------------- | --------- |
| Cost per bit        | Decreases |
| Capacity            | Increases |
| Access Time         | Increases |
| Frequency of Access | Decreases |

---

## Principle of Locality

โปรแกรมมักเข้าถึงข้อมูลแบบ **cluster**

ประเภทของ locality

* **Temporal Locality** – ใช้ข้อมูลเดิมซ้ำ
* **Spatial Locality** – ใช้ข้อมูลใกล้กัน

---

# 4.2 Cache Memory Principles

Cache memory ถูกออกแบบมาเพื่อรวมข้อดีของ

* Memory ที่ **เร็ว**
* Memory ที่ **มีความจุสูง**

```
CPU → Cache → Main Memory
```

---

## Cache Operation

เมื่อ CPU ต้องการข้อมูล

### Cache Hit

ข้อมูลอยู่ใน cache → ส่งให้ CPU ทันที

### Cache Miss

ไม่พบข้อมูล → โหลด block จาก main memory → ส่งให้ CPU

---

## Multi-Level Cache

```
CPU
 ↓
L1 Cache
 ↓
L2 Cache
 ↓
L3 Cache
 ↓
Main Memory
```

| Level | Speed   | Size    |
| ----- | ------- | ------- |
| L1    | Fastest | Small   |
| L2    | Medium  | Larger  |
| L3    | Slower  | Largest |

---

# 4.3 Elements of Cache Design

องค์ประกอบสำคัญในการออกแบบ cache

* Cache Address
* Cache Size
* Mapping Function
* Replacement Algorithm
* Write Policy
* Line Size
* Number of Caches

---

## Cache Address

### Logical Cache

ใช้ **Virtual Address**

ข้อดี

* access เร็ว

ข้อเสีย

* ต้อง flush cache เมื่อ context switch

---

### Physical Cache

ใช้ **Physical Address** หลังจาก MMU แปลง address

---

## Cache Size

Cache ต้องสมดุลระหว่าง

* Speed
* Cost
* Hit Rate

---

## Mapping Function

### Direct Mapping

```
i = j mod m
```

ข้อดี

* Simple
* Fast

ข้อเสีย

* Thrashing

---

### Associative Mapping

Block สามารถเก็บใน line ใดก็ได้

ข้อดี

* Hit rate สูง

ข้อเสีย

* Hardware ซับซ้อน

---

### Set-Associative Mapping

ผสมระหว่าง direct และ associative

```
m = v × k
```

Where

* `m` = cache lines
* `v` = sets
* `k` = lines per set

ตัวอย่าง

* 2-way set associative
* 4-way set associative

---

## Replacement Algorithms

เมื่อ cache เต็ม

* **LRU (Least Recently Used)**
* **FIFO (First In First Out)**
* **LFU (Least Frequently Used)**
* **Random Replacement**

---

## Write Policy

### Write Through

เขียนทั้ง cache และ main memory

ข้อดี

* data consistent

ข้อเสีย

* memory traffic สูง

---

### Write Back

เขียนเฉพาะ cache ก่อน

เขียนกลับ memory ตอน replace

ข้อดี

* ลด memory traffic

ข้อเสีย

* memory อาจไม่ update ทันที

---

## Line Size

Block ที่โหลดเข้า cache

ค่าที่นิยม

```
8 – 64 bytes
```

---

## Number of Caches

### Multilevel Cache

```
L1
L2
L3
```

ช่วยลด **Average Memory Access Time**

---

### Unified vs Split Cache

#### Unified Cache

เก็บ instruction และ data ใน cache เดียว

#### Split Cache

แยกเป็น

* Instruction Cache
* Data Cache

ข้อดี

* ลด contention
* เหมาะกับ pipeline CPU

---

# 4.4 Pentium 4 Cache Organization

Pentium 4 ใช้ **3-Level Cache**

```
L1 Cache
L2 Cache
L3 Cache
```

---

## L1 Cache

| Cache Type        | Size           |
| ----------------- | -------------- |
| Instruction Cache | ~12K micro-ops |
| Data Cache        | 16 KB          |

* 4-way set associative
* 64-byte line size

---

## L2 Cache

* Size ≈ **512 KB**
* 8-way set associative
* 128-byte line size

---

## L3 Cache

ใช้ในรุ่น high-end

* Size ≈ **1 MB**
* เพิ่ม performance สำหรับ data ขนาดใหญ่

---

## Pentium 4 Core Components

1. Fetch / Decode Unit
2. Out-of-Order Execution Logic
3. Execution Units
4. Memory Subsystem

---

# 4.5 Key Terms

คำศัพท์สำคัญในบทนี้

* Cache Memory
* Memory Hierarchy
* Locality of Reference
* Cache Hit
* Cache Miss
* Direct Mapping
* Associative Mapping
* Set-Associative Mapping
* Replacement Algorithm
* Write Through
* Write Back
* Multilevel Cache
