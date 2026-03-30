# Appendix 4A: Performance Characteristics of Two-Level Memories

**ประสิทธิภาพของระบบหน่วยความจำ 2 ระดับ** - สูตรคำนวณและการวิเคราะห์ Cache Performance

---

## 🎯 ภาพรวม Two-Level Memory
Level 1 (Cache): เล็ก+เร็ว
Level 2 (Main Memory): ใหญ่+ช้า

**เป้าหมาย**: **ลด Average Access Time** โดยใช้ Hit Ratio สูง

---

## 📊 **สูตรคำนวณ Effective Access Time (EAT)**

### **กรณีทั่วไป**
EAT = h × t1 + (1-h) × (t1 + t2)
**h** = Hit Ratio (สัดส่วนที่เจอใน Cache)  
**t1** = Cache Access Time  
**t2** = Main Memory Access Time  

### **ทำไมต้องมี t1 ใน Miss?**
Miss Case: Cache Check (t1) + Main Memory (t2)

---

## 🔢 **ตัวอย่าง 4.1** - คำนวณ EAT
Level 1: 1000 words, t1 = 0.01ms
Level 2: 100K words, t2 = 0.1ms
Hit Ratio h = 95% = 0.95

EAT = 0.95 × 0.01 + (1-0.95) × (0.01 + 0.1)
= 0.95 × 0.01 + 0.05 × 0.11
= 0.0095 + 0.0055
= 0.015ms

**ผลลัพธ์**:
Pure Cache: 0.01ms
Pure Main: 0.1ms
Hybrid: 0.015ms ← ใกล้ Cache มาก!

---

## 📈 **กราฟประสิทธิภาพ** (รูปที่ 4.2)
Average Access Time (ms)
0.1 | ┌───────────── T2 (Main Memory)
| │
0.05| ┌───┘
| │
0.01|┌───┘ T1 (Cache)
|│
0.0 +-------------------1.0
0 Hit Ratio (h)

**ลักษณะสำคัญ**:
- **h = 0%**: EAT = t1 + t2 = 0.11ms
- **h = 100%**: EAT = t1 = 0.01ms  
- **h = 95%**: EAT = 0.015ms (**ดีมาก!**)

---

## 🔍 **Hit Ratio (h)**
h = จำนวน Hit / (Hit + Miss)
= จำนวนครั้งที่เจอใน Cache / ทุกครั้งที่เข้าถึง

### **ปัจจัยที่ทำให้ Hit Ratio สูง**
1. **Locality of Reference** (หลักการสำคัญ)
2. **Cache Size** ใหญ่พอ
3. **Set-Associative Mapping** (2-4 way)
4. **LRU Replacement**

---

## ⚙️ **Miss Types** (3 ประเภท)

| **Miss Type** | **สาเหตุ** | **วิธีลด** |
|---------------|------------|------------|
| **Compulsory** | ครั้งแรกที่เข้าถึง | **Prefetching** |
| **Capacity** | Cache เล็กเกิน | **Cache Size ↑** |
| **Conflict** | Direct Mapping | **Set-Associative** |

---

## 📋 **สูตรขั้นสูง**

### **1. Average Access Time กับ Miss Penalty**
EAT = t1 + (1-h) × t2
t2 = Miss Penalty

### **2. Multi-Level Cache**
EAT = h1×t1 + (1-h1)×[h2×t2 + (1-h2)×(t2+t3)]

**ตัวอย่าง 3-Level**:
L1: h1=98%, t1=1ns
L2: h2=90%, t2=10ns
L3: t3=100ns
EAT ≈ 1.9ns (ดีมาก!)

---

## 🎯 **การคำนวณจริง**

### **สมมติ Intel Core i7**
L1: h1 = 95%, t1 = 4 cycles
L2: h2 = 85%, t2 = 12 cycles
L3: h3 = 70%, t3 = 40 cycles
Clock = 3GHz (0.33ns/cycle)

EAT = 4 + (1-0.95)×[12 + (1-0.85)×[40 + (1-0.70)×100]]
≈ 4 + 0.6 + 3.0 + 16.2
≈ 23.8 cycles ≈ 7.8ns

---

## 💡 **Key Insights**
Hit Ratio > 90% = สำคัญมาก!

t1 << t2 → Cache ช่วยเยอะ

Multi-Level → EAT ใกล้ t1 สุดๆ

h ↓ 10% → EAT ↑ 2-3 เท่า!

**กรณีเลวร้าย**:
h = 50%, t1=1ns, t2=100ns
EAT = 1 + 0.5×100 = 51ns (แย่มาก!)

---

## 🎯 **สรุป Appendix 4A**
✅ สูตรหลัก: EAT = h×t1 + (1-h)×(t1+t2)
✅ Hit Ratio 90-95% = เป้าหมายสำคัญ
✅ Multi-Level Cache → Performance ดีสุด
✅ Locality = รากฐานความสำเร็จ

"Cache Performance = Hit Ratio × Speed Difference"
**เป้าหมาย**: **เข้าใจสูตรและปัจจัยที่กำหนด Cache Performance** [file:11]


