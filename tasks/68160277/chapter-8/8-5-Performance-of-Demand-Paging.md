# 8.5 Performance of Demand Paging

**ประสิทธิภาพของ Demand Paging** - วัดผลและ Optimize Page Fault Rate

---

## 🎯 **Effective Access Time (EAT) Formula**
EAT = (1-p) × t_m + p × t_f
**p** = Page Fault Rate  
**t_m** = Memory Access Time (~100ns)  
**t_f** = Page Fault Overhead (~8-25ms)

### **ตัวอย่างคำนวณ**
t_m = 100ns, t_f = 10ms, p = 0.001% (1/100K)
EAT = 0.99999×100ns + 0.00001×10ms
= 99.999ns + 100μs
= ~200μs ← ลด 2000x!

---

## 📈 **Page Fault Rate (p) Impact**

| **p** | **EAT** | **Performance** |
|-------|---------|-----------------|
| **0%** | 100ns | Perfect |
| **0.001%** | ~200μs | **Acceptable** |
| **0.01%** | **2ms** | Poor |
| **0.1%** | **10ms** | **Thrashing!** |

**Goal**: **p < 0.001%** (1 fault / 100K accesses)

---

## 🧠 **Working Set Model**
Working Set (WS) = Pages ที่ Process ใช้ในช่วง τ
τ = Working Set Window (เช่น k instructions)

### **Working Set Algorithm**
Track Pages ใน Window τ

Allocate |WS| + α Frames

ถ้า |WS| > Frames → Page Fault ↑

**ตัวอย่าง**:
Process A: WS = {Page 10,15,20,25} → 4 Frames
Frames = 6 → OK
Frames = 3 → Thrashing!

---

## 📊 **Thrashing Phenomenon**
Degree of Multiprogramming (DOP) ↑ → Performance?

CPU Utilization:
100% │ ┌──┐ Peak
│ ┌──┘ │
50% │ ┌──┘ │
│ ┌──┘ │
0% └─────────────┼───→ DOP
Optimal Thrashing

**Thrashing** = **p → 100%** (ทุก Access = Fault)

---

## 🔧 **Thrashing Prevention**

### **1. Working Set Model**
Monitor WS Size → Allocate Frames = |WS| + α

### **2. Page Fault Frequency (PFF)**
PFF > Threshold → ลด DOP

### **3. Local Replacement**
Process A Fault → Replace A’s Page เท่านั้น
≠ Global (A กระทบ B)

---

## 📈 **Local vs Global Replacement**

| **Global** | **Local** |
|------------|-----------|
| Fault A → Replace ทุก Process | Fault A → Replace A เท่านั้น |
| **Simple** | **Fair** |
| **Interference** | No Interference |

**Linux**: **Per-Process LRU** (Local-like)

---

## ⚙️ **Program Behavior Models**

### **1. Phase Behavior**
Program มี Phase ต่างกัน:
Phase 1: WS = 10 Pages
Phase 2: WS = 50 Pages
Phase 3: WS = 5 Pages

### **2. Loop Behavior**
for(i=0;i<N;i++) {
array[i] = compute();
}
→ WS = Array Pages ใน Loop

---

## 📊 **Real System Performance**
VAX/VMS (1980s):
Frames | DOP | Fault Rate | CPU Util
────────┼─────┼────────────┼─────────
64 | 2 | 0.1% | 90%
128 | 4 | 0.5% | 85%
256 | 8 | 5.0% | 20% ← Thrashing!

---

## 🔢 **EAT Calculation Examples**
System: t_m = 200ns, t_f = 12ms

PFR | DOP | EAT | Comment
───────┼─────┼─────────┼────────────────
0.0001%│ 2 │ 2.4μs │ Excellent
0.001% │ 4 │ 24μs │ Good
0.01% │ 6 │ 240μs│ Poor
0.1% │ 8 │ 1.2ms│ Thrashing

---

## 🚀 **Modern Optimizations**

### **1. Super Pages (Large Pages)**
4KB → 2MB/1GB Pages
TLB Entries ↓ → TLB Hit ↑
Page Fault ↓ (ใหญ่กว่า)

### **2. Transparent Huge Pages (THP)**
Linux Auto: 4KB → 2MB เมื่อ Sequential Access

### **3. NUMA Awareness**
Allocate Pages ใกล้ CPU (Local Memory)

---

## 🎯 **สรุป 8.5 Performance**
✅ EAT = (1-p)×tm + p×tf ← สูตรสำคัญ
✅ Goal: p < 0.001% (1/100K accesses)
✅ Working Set: |WS| + α Frames
✅ Thrashing: DOP สูง → p → 100%
✅ Prevention: WS Monitor + PFF + Local Replacement

"Page Fault Rate = System Performance #1 Factor!"
**เป้าหมาย**: **p < 0.001% → Performance ดี** [file:11]
