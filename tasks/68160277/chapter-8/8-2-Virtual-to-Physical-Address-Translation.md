# 8.2 Hardware and Control Structures

**โครงสร้างฮาร์ดแวร์และการควบคุมสำหรับ Virtual Memory**

---

## 🏗️ **Address Translation Overview**

Virtual Address (32-bit) → Physical Address (32-bit)
Process "เห็น" 4GB Virtual Space เสมอ

---

## 📋 **Virtual Address Structure**
32-bit Virtual Address:
┌──────────────────────┬──────────────────────┐
│ Virtual Page Number│ Page Offset │
│ (20 bits) │ (12 bits) │ ← 4KB Page
└──────────────────────┴──────────────────────┘
│ │
▼ ▼
Page Table Physical Frame
Lookup (PTE) (Offset เดียวกัน)

**Page Size = 4KB = 2^12 bytes** → Offset = 12 bits  
**VPN = 20 bits** → 1M Pages ใน Virtual Space

---

## 🔍 **Translation Process (Step-by-Step)**

```mermaid
flowchart TD
    A[Virtual Address<br/>0x00123456] --> B[VPN=0x0123<br/>Offset=0x456]
    B --> C[TLB Lookup]
    C --> D{TLB Hit?}
    D -->|YES| E[PFN=0x05A3]
    D -->|NO| F[Page Table Base Register<br/>PTBR=0x1000]
    F --> G[Page Table[VPN=0x0123]]
    G --> H[PTE=PFN=0x05A3, Valid=1]
    H --> E
    E --> I[Physical Addr=<br/>0x05A3456]
```

---

## 🛠️ **Hardware Components**

### **1. Translation Lookaside Buffer (TLB)**
TLB = "Cache ของ Page Table"
Fully Associative, 32-256 entries

| **Field** | **Size** | **Purpose** |
|-----------|----------|-------------|
| **VPN** | 20 bits | Virtual Page Number |
| **PFN** | 18 bits | Physical Frame Number |
| **Valid** | 1 bit | Page ใน Memory? |

**TLB Hit Time**: 1 cycle  
**TLB Miss**: Page Table Walk (100+ cycles)

### **2. Page Table Base Register (PTBR)**
PTBR = จุดเริ่มต้น Page Table ใน Physical Memory
Process เปลี่ยน → PTBR Update

### **3. Page Table**
1M entries × 4 bytes = 4MB/Page Table
Process 4GB → Page Table 4MB

---

## 📊 **Page Table Entry (PTE) Structure**
PTE (4 bytes = 32 bits):
┌──────────────┬──┬──┬──┬──────┐
│ PFN (18 bits)│V │D │R │Prot │
│Physical Frame│ │ │ │R/W/X │
└──────────────┴──┴──┴──┴──────┘

V = Valid (1=ใน Memory, 0=Disk)
D = Dirty (Modified)
R = Reference (Recently Used)
Prot = Protection (Read/Write/Execute)

---

## 🔢 **ตัวอย่างการแปล Address**
Virtual Address: 0x00123456 (1183782 decimal)
Page Size: 4KB

Offset = 0x456 = 1110 bytes

VPN = 0x0123 = 291 decimal

TLB Miss → Page Table

PTE = PFN=0x05A3 (1443), Valid=1

Physical Address = 0x05A3456

---

## ⚠️ **Page Table Problems**

### **1. Page Table ใหญ่เกิน**
4GB Virtual → 1M PTE × 4B = 4MB
64-bit: 2^47 VPN → 1TB Page Table!

### **2. Multi-level Page Table** (แก้ปัญหา)
3-Level Page Table (x86-64):
PML4 (512 entries) → PDPT → PD → PT → Page
Total: 4KB Page Table เท่านั้น!

Virtual Address (48-bit):
┌──┬──────┬──────┬──────────────┐
│PML4│ PDPT │ PD │PT + Offset │
│ 9b│ 9b │ 9b │ 21 bits │
└──┴──────┴──────┴──────────────┘

---

## 🚀 **TLB Organization**
TLB = Fully Associative Cache ของ Page Table
Hit Rate: 95-99%

Hardware TLB:

Data TLB (DTLB): Data Access

Instruction TLB (ITLB): Instruction Fetch

**TLB Entry**:
[VPN][PFN][Valid][Protection][ASID]
ASID = Address Space ID (Process ID)

---

## 📈 **Performance Impact**
Memory Access Time:
No TLB + Page Table Walk = 100-1000 cycles!
TLB Hit = 1 cycle
TLB Miss Rate 1% → Performance ↓ 10x!

---

## 🎯 **สรุป 8.2 Hardware Structures**
✅ Virtual Addr = VPN + Offset
✅ TLB = Page Table Cache (Critical!)
✅ Page Table = VPN → PFN + Control Bits
✅ PTBR = Page Table Location
✅ Multi-level PT = ลด Memory Overhead
✅ PTE: PFN + Valid + Dirty + Reference + Protection

"TLB = Performance Bottleneck #1 ของ Virtual Memory!"
**เป้าหมาย**: **Address Translation เร็ว + Page Table จัดการง่าย** [file:11]
