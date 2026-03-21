# 📘 Chapter 4: Cache Memory

## 📌 4.1 Computer Memory System Overview

### 🔹 ภาพรวมระบบหน่วยความจำ
- คอมพิวเตอร์มีหน่วยความจำหลายประเภท → ใช้ร่วมกันเป็น **Memory Hierarchy**
- แบ่งเป็น:
  - **Internal Memory**: Register, Cache, Main Memory
  - **External Memory**: Disk, Tape

### 🔹 Characteristics of Memory Systems
- Location: Internal / External  
- Capacity: Byte / Word  
- Unit of Transfer: Word / Block  
- Access Method: Sequential, Direct, Random, Associative  
- Performance: Access Time, Cycle Time, Transfer Rate  
- Physical Type: Semiconductor, Magnetic, Optical  
- Characteristics: Volatile / Non-volatile  
- Organization: Memory modules  

### 🔹 Memory Hierarchy
1. Register  
2. Cache  
3. Main Memory  
4. Disk  
5. Tape  

- ความเร็ว ↑ → ราคา ↑ → ขนาด ↓  
- ใช้หลัก **Locality of Reference**

---

## 📌 4.2 Cache Memory Principles

### 🔹 แนวคิด Cache
- Cache = หน่วยความจำขนาดเล็ก แต่เร็วมาก
- เก็บข้อมูลที่ใช้บ่อยจาก Main Memory

### 🔹 การทำงาน
1. CPU ขอข้อมูล  
2. ตรวจสอบ Cache  
   - Hit → ใช้ได้ทันที  
   - Miss → โหลดจาก Main Memory  

### 🔹 Multi-level Cache
- L1 → เร็วที่สุด  
- L2 → ใหญ่ขึ้น  
- L3 → ใหญ่และช้าลง  

---

## 📌 4.3 Elements of Cache Design

### 🔹 Cache Size
- เล็ก → เร็ว  
- ใหญ่ → Hit rate สูง  

### 🔹 Mapping Function
- Direct Mapping → ง่ายแต่ชนบ่อย  
- Associative → ยืดหยุ่นแต่ซับซ้อน  
- Set-Associative → นิยมใช้  

### 🔹 Replacement Algorithms
- LRU (นิยมที่สุด)  
- FIFO  
- LFU  
- Random  

### 🔹 Write Policy
- Write Through → ปลอดภัยแต่ช้า  
- Write Back → เร็วแต่ซับซ้อน  

### 🔹 Line Size
- ใหญ่ → miss ลด  
- ใหญ่เกิน → เปลือง  

### 🔹 Number of Caches
- Multi-level (L1, L2, L3)  
- Split vs Unified Cache  

---

## 📌 4.4 Pentium 4 Cache Organization

- ใช้ Multi-level Cache  
- มี L1, L2, L3  
- เพิ่ม performance ด้วย cache หลายระดับ  

---

## 📌 Key Takeaways
- Cache ช่วยเพิ่มความเร็วระบบ  
- ใช้ Locality เพิ่ม efficiency  
- การออกแบบ cache มีผลต่อ performance มาก
