# 📄 4-3-Elements-of-Cache-Design.md

# 4.3 Elements of Cache Design

**องค์ประกอบการออกแบบ Cache** - พารามิเตอร์หลัก 6 ตัวที่กำหนดประสิทธิภาพ Cache

---

## 📋 **Cache Design Elements** (ตาราง 4.2)

| **Design Element** | **ตัวเลือก** | **ผลกระทบ** |
|--------------------|--------------|-------------|
| **Cache Addresses** | Logical/Physical | Speed vs Security |
| **Cache Size** | 8KB-หลาย MB | Cost vs Hit Ratio |
| **Mapping Function** | **Direct/Associative/Set-Associative** | Speed vs Flexibility |
| **Replacement Algorithm** | **LRU/FIFO/LFU/Random** | Hit Ratio |
| **Write Policy** | **Write-through/Write-back** | Speed vs Consistency |
| **Line Size** | 32-256 bits | Spatial Locality vs Compulsory Miss |
| **# of Caches** | L1/L2/L3 | Performance Hierarchy |

---

## 1. **Cache Addresses**

### **Logical Cache (Virtual Cache)**
Processor → Cache → MMU → Main Memory

**✅ ข้อดี**: เร็ว (ไม่ต้องแปล Virtual→Physical)  
**❌ ข้อเสีย**: Context Switch ต้อง Flush Cache

### **Physical Cache**
Processor → MMU → Cache → Main Memory
**✅ ข้อดี**: ปลอดภัย (ไม่ Flush ตอน Context Switch)  
**❌ ข้อเสีย**: ช้ากว่า (MMU ก่อน)

---

## 2. **Cache Size**
ใหญ่ = Hit Ratio ↑ | Cost ↑ | ช้ากว่า (Address Decoding)
เล็ก = Cost ↓ | Hit Ratio ↓ | เร็วกว่า

**ตารางขนาด Cache** (ตาราง 4.3):
Pentium: L1=8KB, L2=256-512KB
PowerPC: L1=32KB, L2=256KB-1MB

---

## 3. **Mapping Functions** ⭐ **สำคัญที่สุด**

### **A. Direct Mapping**
i = j mod m
Block j → Line i เดียวเท่านั้น

**ตัวอย่าง**:
Cache: 16K lines, Block: 4 bytes
Address 24-bit = [8-bit Tag][14-bit Line][2-bit Word]
Cache Line #0: Blocks 0, 16K, 32K, ...
Cache Line #1: Blocks 1, 16K+1, 32K+1, ...

**Pros**: **ง่าย ถูก เร็ว**  
**Cons**: **Thrashing** (Conflict Miss เยอะ)

**รูปที่ 4.9**: Address = Tag + Line + Word

### **B. Fully Associative**
Block j → ทุก Line ใน Cache

**Address**: `[Tag][Word]` (ไม่มี Line field)  
**การทำงาน**: เปรียบเทียบ Tag ทุก Line พร้อมกัน

**Pros**: **Hit Ratio สูงสุด**  
**Cons**: **Hardware ซับซ้อน ช้า แพง**

### **C. Set-Associative** ⭐ **ใช้จริง**
i = j mod v
Block j → Set i (มี k lines/set)

**ตัวอย่าง 2-way Set-Associative**:
Address 24-bit = [9-bit Tag][13-bit Set][2-bit Word]
Set 0: 2 lines, Set 1: 2 lines, ...

**รูปที่ 4.14**: k-way Set-Associative Organization

---

## 4. **Performance Comparison** (รูปที่ 4.16)
Hit Ratio vs Cache Size:
64KB: Direct(0.85) < 2-way(0.92) < 4-way(0.94)
4KB: Direct(0.75) < 2-way(0.88) << 4-way(0.90)

**สรุป**: **2-4 way = สมดุลดีที่สุด**

---

## 5. **Replacement Algorithms**

| **Algorithm** | **หลักการ** | **Hardware** | **ประสิทธิภาพ** |
|---------------|-------------|--------------|------------------|
| **LRU** | ไม่ใช้มานานสุด | ซับซ้อน | **ดีสุด** |
| **FIFO** | เข้ามานานสุด | ง่าย | ปานกลาง |
| **LFU** | ใช้น้อยสุด | ปานกลาง | ดี (Workload เฉพาะ) |
| **Random** | สุ่ม | **ง่ายสุด** | ปานกลาง |

**LRU 2-way**: ใช้ 1-bit (USE bit) สลับกัน

---

## 6. **Write Policies**

### **Write-through**
CPU Write → Cache + Main Memory (พร้อมกัน)
**✅ ปลอดภัย** (Main Memory ตรงกับ Cache เสมอ)  
**❌ ช้า** (ต้องเขียน 2 ที่)

### **Write-back**
CPU Write → Cache เท่านั้น (Dirty Bit = 1)
Cache → Main Memory (ตอน Replace หรือ Flush)
**✅ เร็ว** (เขียน 1 ที่)  
**❌ เสี่ยง** (Main Memory อาจเก่า)

---

## 7. **Line Size**
เล็ก (32B): Compulsory Miss ↑, Spatial Locality ↓
ใหญ่ (128B): Compulsory Miss ↓, Conflict Miss ↑
Optimal: 32-64 bytes

---

## 🎯 **สรุป Elements of Cache Design**
🏆 Best Practices (สมัยใหม่):
├── 2-4 way Set-Associative
├── LRU Replacement
├── Write-back Policy
├── 32-64B Line Size
├── L1+L2+L3 Hierarchy
└── Physical Addressing
Trade-off:
Direct = เร็ว/ถูก → Hit Ratio ต่ำ
Associative = Hit Ratio สูง → ช้า/แพง
Set-Associative = สมดุลดีที่สุด

**เป้าหมาย**: **Hit Ratio สูง + Hardware เรียบง่าย** [file:11]
