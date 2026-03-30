# 4.4 Pentium 4 Cache Organization

**การจัดระเบียบ Cache ใน Intel Pentium 4** - ตัวอย่าง Cache Hierarchy ขั้นสูงในโปรเซสเซอร์จริง

---

## 🏗️ **Pentium 4 Cache Hierarchy**
Pentium 4 Core
│
┌───────────────────┼───────────────────┐
│ │ │
L1 I-Cache L1 D-Cache L2 Cache (Unified)
12K µops 8KB, 4-way 256KB-2MB, 8-way
64B lines 64B lines 128B lines

### **รายละเอียดแต่ละระดับ**

| **Cache Level** | **ขนาด** | **Associativity** | **Line Size** | **Access Time** | **Unified/Split** |
|-----------------|----------|-------------------|---------------|-----------------|-------------------|
| **L1 Instruction** | **12K µops** | 8-way | 64B | **3 cycles** | **Instruction** |
| **L1 Data** | **8KB** | **4-way** | **64B** | **3 cycles** | **Data** |
| **L2** | **256KB-2MB** | **8-way** | **128B** | **14 cycles** | **Unified** |
| **L3** (Optional) | 1-4MB | 4-way | 128B | 30+ cycles | Unified |

---

## 🚀 **Advanced Features ของ Pentium 4**

### **1. Trace Cache (L1 Instruction Cache)**
≠ ค่าเดิม Instruction Cache
Trace Cache = เก็บ "µops" (Micro-operations) ที่ Decode แล้ว
**ข้อดี**: 
- ลด Decode Stage
- เก็บ Instruction sequence ที่ใช้บ่อย (Locality)
- **12K µops** (≠ 12KB)

### **2. Advanced Dynamic Execution**
20-stage pipeline (ยาวมาก)

Trace Cache + Branch Prediction

Out-of-order Execution

### **3. Non-blocking Cache**
L1 D-Cache สามารถ:

รับ Multiple Outstanding Misses

ไม่รอ Miss เสร็จ → ดำเนินการต่อ

### **4. Advanced Branch Prediction**
2-level Adaptive Predictor +
Trace Cache Branch History

---

## 📊 **Cache Parameters (Northwood 2.4GHz)**
L1 I-Cache: 12K µops, 8-way set-associative, 64B lines
L1 D-Cache: 8KB, 4-way set-associative, 64B lines
L2 Cache: 512KB, 8-way set-associative, 128B lines

Advanced Transfer Cache (ATC):

L2 → L1 transfer เร็วขึ้น 72 bytes/cycle

---

## 🔍 **Cache Addressing**
Physical Addressing (ทุกระดับ)
32-bit Physical Address = [Tag][Index][Offset]

L1 D-Cache (8KB, 64B lines, 4-way):

128 lines/set = 7-bit Index

64B line = 6-bit Offset

Tag = 32 - 7 - 6 = 19 bits

---

## ⚙️ **Write Policy & Coherency**

L1 Data: Write-back + Write-allocate
L2 Cache: Write-back

Cache Coherency:

MESI Protocol (Modified/Exclusive/Shared/Invalid)

Snooping + Directory

---

## 📈 **Performance Characteristics**
Hit Ratios (ประมาณ):
L1: 95-98%
L2: 85-90%
Overall: ~96%

Pipeline: 20 stages → Cache Miss แพงมาก!
→ L1 Hit = Critical

---

## 🎯 **เปรียบเทียบกับ Cache ทั่วไป**

| **Feature** | **Pentium 4** | **ทั่วไป** |
|-------------|---------------|------------|
| **L1 I-Cache** | **Trace Cache (12K µops)** | Instruction (16-32KB) |
| **L1 Split** | **Yes** | Yes/No |
| **L2 Size** | **256KB-2MB** | 256KB-1MB |
| **Associativity** | **4-8 way** | 2-4 way |
| **Pipeline** | **20 stages** | 10-15 stages |

---

## 💡 **Key Innovations**
Trace Cache → ลด Decode Bottleneck

Deep Pipeline (20 stages) → High Clock Speed

Large L2 (512KB-2MB) → ชดเชย Miss Penalty

Advanced Branch Prediction → ลด Control Hazard

Non-blocking L1 → Multiple Misses

---

## 🎯 **สรุป Pentium 4 Cache Organization**
✅ Split L1 (I+D) + Large Unified L2
✅ Trace Cache = Innovation สำคัญ
✅ High Associativity (4-8 way) = Hit Ratio สูง
✅ Physical Addressing + Write-back
✅ MESI Coherency = Multi-processor Ready

🏆 จุดเด่น: Balance ระหว่าง Clock Speed สูง + Cache Performance

**เป้าหมาย**: แสดง**Cache Design ขั้นสูง**ในโปรเซสเซอร์จริง [file:11]

