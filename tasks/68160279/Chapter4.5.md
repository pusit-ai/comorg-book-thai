# 4.5 Multilevel Caches and Case Study

## Multilevel Caches (Cache หลายระดับ)
* **On-chip Cache (L1):** อยู่ในตัวชิป Processor ช่วยลดการใช้งาน Bus ภายนอกและทำงานเร็วที่สุด
* **L2 / L3 Cache:** มักมีขนาดใหญ่กว่า L1 เพื่อช่วยลดอัตราการเข้าถึง Main Memory (DRAM) ที่ช้ากว่ามาก
* การมี Cache หลายระดับช่วยเพิ่มประสิทธิภาพภาพรวมได้ดีกว่าการมีระดับเดียวขนาดใหญ่

## Unified vs. Split Caches
* **Unified Cache:** เก็บทั้งคำสั่ง (Instruction) และข้อมูล (Data) ใน Cache เดียวกัน ข้อดีคือปรับสมดุลภาระงานได้เอง
* **Split Cache:** แยก Cache สำหรับคำสั่งและข้อมูลออกจากกัน ข้อดีคือลดการแย่งชิงทรัพยากร (Contention) ระหว่างหน่วย Fetch และหน่วย Execution เหมาะกับระบบ Pipeline

## กรณีศึกษา: Pentium 4
* วิวัฒนาการจาก 80386 (ไม่มี On-chip) มาจนถึง Pentium 4 ที่มี L1, L2 และ L3
* **L1 Data Cache:** 16 kB, 4-way set-associative
* **L1 Instruction Cache:** ออกแบบพิเศษเพื่อนั่งอยู่ระหว่างหน่วย Decode และ Execution โดยเก็บเป็น Micro-operations (μops) แทนคำสั่งปกติ เพื่อลดความซับซ้อนในการทำงาน
* สนับสนุนโหมดการทำงานทั้ง Write-back และ Write-through ผ่านการตั้งค่า Control Bits