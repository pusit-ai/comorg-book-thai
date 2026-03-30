# 8.4 Page Replacement Algorithms

**อัลกอริทึมการแทนที่ Pages** - เลือก Page ไหนให้ออกจาก Memory เมื่อ Frames เต็ม

---

## 🎯 **ปัญหา Page Replacement**
Frames เต็ม → ต้องเลือก 1 Page ออกจาก Memory
Page ที่เลือกผิด → Page Fault ถัดไปทันที!

**เป้าหมาย**: **ลด Page Fault Rate (p)**

---

## 📊 **อัลกอริทึมหลัก 5 แบบ**

| **Algorithm** | **หลักการ** | **Optimal?** | **Hardware** | **ใช้จริง** |
|---------------|-------------|--------------|--------------|-------------|
| **Optimal** | รู้อนาคต → เปลี่ยน Page ที่ไม่ใช้**นานสุด** | **ดีสุด** | ❌ Impossible | ทฤษฎี |
| **FIFO** | เข้ามานานสุด | ❌ | ✅ ง่าย | น้อย |
| **LRU** | ไม่ใช้มานาน** | ✅ | ❌ ซับซ้อน | ✅ |
| **LFU** | ใช้**น้อยสุด** | ✅ | ปานกลาง | น้อย |
| **Clock** | **Reference Bit** | ✅ | **ง่าย** | **มาก** |

---

## 🔢 **ตัวอย่าง: Reference String**
Ref String: 7,0,1,2,0,3,0,4,2,3,0,3,2,1,2,0,1,7,0,1
Frames: 3 หรือ 4

### **1. Optimal Algorithm** (Belady's Optimal)
Frames: 3 → 8 Faults
Ref | Frames | Fault
-----+──────────────────────+─────
7 | 7 | ✓
0 | 7 0 | ✓
1 | 7 0 1 | ✓
2 | 7 0 1 2 (เต็ม) | ✓
0 | 7 0 1 2 |
3 | 7 0 1 3 (2→3) | ✓ ← 2 ไม่ใช้ 3 ครั้ง
0 | 7 0 1 3 |
4 | 7 0 1 4 (3→4) | ✓ ← 3 ใช้ครั้งสุดท้าย
...
Total: 8 Faults

### **2. FIFO (First-In-First-Out)**
Frames: 3 → 9 Faults | Frames: 4 → 10 Faults ❌ Belady's Anomaly!

เต็ม → เปลี่ยน Page แรกสุดที่เข้า (ไม่สนใช้หรือไม่)

### **3. LRU (Least Recently Used)**
Frames: 3 → 10 Faults

เปลี่ยน Page ที่ไม่ใช้มานานสุด
Hardware: Stack/Counter ทุก Page

---

## 🕰️ **Clock Algorithm** (ใช้จริงมากสุด!)
"Approximate LRU" + ง่าย

### **Circular List + Reference Bit**
Frames ในวงกลม → Reference Bit (R=1 เมื่อใช้)

Clock Hand หมุน:

R=0 → Replace Page นี้

R=1 → R=0, Hand หมุนต่อ

**ตัวอย่าง**:
Frames: [7:R1][0:R1][1:R0][2:R1] ← Hand ที่ 1
Ref 3:

Frame#1 (R=0) → Replace → [7:R1][0:R1][3:R1][2:R1]

**Pros**: **Hardware ง่าย (1 bit + Pointer)**  
**Cons**: ไม่ Perfect LRU

---

## 📈 **เปรียบเทียบ Performance**
Reference String: 7,0,1,2,0,3,0,4,2,3,0,3,2,1,2,0,1,7,0,1
Frames = 3

Algo | Faults | Notes
────────┼────────┼──────────────────
Optimal | 7 | รู้อนาคต
FIFO | 9 | Belady's Anomaly
LRU | 10 |
LFU | 11 |
Clock | 9 | Practical!
Random | 12 |

---

## 🔍 **Belady's Anomaly** (FIFO ปัญหา)
FIFO Frames=3: 9 Faults
FIFO Frames=4: 10 Faults ← เพิ่ม Frame กลับเพิ่ม Fault!

Optimal/LRU: Frames ↑ → Faults ↓ เสมอ

---

## ⚙️ **Second Chance Algorithm** (= Clock)
ถ้า R=1 → R=0 + Second Chance

ถ้า R=0 → Replace

Ref 3, Frames: [7:R1][0:R1][1:R0]

Frame#1 R=0 → Replace → [7:R1][0:R1][3:R1]

---

## 🛠️ **Hardware Implementation**

### **LRU Hardware** (ซับซ้อน)
N Frames → N-bit Counter ทุก Frame
ทุก Access → Update Counters ทุก Frame

### **Clock Hardware** (ง่าย)
Circular Linked List

1-bit Reference ทุก Frame

Hand Pointer

---

## 📊 **Modern Systems**
Linux: Clock-Pro (Enhanced Clock)
Windows: Demand Paging + Working Set

**Clock-Pro**:
Use Reference + Dirty Bits

Evict Cold Pages (R=0 นาน)

---

## 🎯 **สรุป 8.4 Page Replacement**
🏆 Clock Algorithm = ใช้จริงมากสุด
├── Hardware ง่าย (1 bit + Pointer)
├── Performance ใกล้ LRU
└── ไม่มี Belady's Anomaly

❌ FIFO = Belady's Anomaly
✅ Optimal = ทฤษฎีเท่านั้น
✅ LRU = ดีแต่ Hardware แพง

"Clock = Practical Solution สำหรับ Real Systems!"
**เป้าหมาย**: **ลด Page Faults ด้วย Algorithm ง่าย+มีประสิทธิภาพ** [file:11]
