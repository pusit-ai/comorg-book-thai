chapter 5.6 Key Terms, Review Questions, and Problems
🔹 🔑 Key Terms (คำศัพท์สำคัญ)
| คำศัพท์                    | ความหมาย (ภาษาไทย)                              |
| -------------------------- | ----------------------------------------------- |
| Semiconductor memory       | หน่วยความจำที่สร้างจากวงจรเซมิคอนดักเตอร์       |
| RAM (Random Access Memory) | หน่วยความจำที่เข้าถึงข้อมูลตำแหน่งใดก็ได้โดยตรง |
| DRAM                       | หน่วยความจำแบบไดนามิก ต้อง refresh              |
| SRAM                       | หน่วยความจำแบบสแตติก เร็ว ไม่ต้อง refresh       |
| ROM                        | หน่วยความจำอ่านอย่างเดียว                       |
| PROM                       | ROM ที่เขียนได้ 1 ครั้ง                         |
| EPROM                      | ROM ที่ลบด้วยแสง UV                             |
| EEPROM                     | ROM ที่ลบด้วยไฟฟ้า                              |
| Flash memory               | หน่วยความจำลบเป็น block                         |
| Nonvolatile memory         | หน่วยความจำที่ไม่หายเมื่อปิดไฟ                  |
| NAND flash                 | Flash แบบ block access                          |
| NOR flash                  | Flash แบบ random access                         |
| DDR DRAM                   | DRAM ที่ส่งข้อมูลได้ 2 ครั้งต่อ clock           |
| MRAM                       | หน่วยความจำใช้สนามแม่เหล็ก                      |
| PCRAM                      | หน่วยความจำใช้การเปลี่ยนสถานะของวัสดุ           |
| ReRAM                      | หน่วยความจำใช้ค่าความต้านทาน                    |
| ECC                        | การตรวจจับและแก้ไขข้อผิดพลาด                    |
| Hamming code               | วิธีตรวจและแก้ error แบบหนึ่ง                   |
| SEC                        | แก้ error ได้ 1 bit                             |
| SEC-DED                    | แก้ 1 bit + ตรวจ 2 bit                          |
| Hard failure               | ความเสียหายถาวร                                 |
| Soft error                 | error ชั่วคราว                                  |

🔹 📌 Review Questions (คำถามทบทวน + สรุปคำตอบสั้น)
| ข้อ  | คำถาม                             | สรุปคำตอบ                          |
| ---- | --------------------------------- | ---------------------------------- |
| 5.1  | คุณสมบัติของ semiconductor memory | เก็บ 0/1, อ่าน/เขียนได้            |
| 5.2  | ความหมาย RAM 2 แบบ                | (1) เข้าถึงสุ่ม (2) อ่านเขียนได้   |
| 5.3  | DRAM vs SRAM (การใช้งาน)          | DRAM = main memory, SRAM = cache   |
| 5.4  | DRAM vs SRAM (ลักษณะ)             | DRAM ช้า/ถูก, SRAM เร็ว/แพง        |
| 5.5  | ทำไม DRAM เป็น analog             | ใช้ประจุ capacitor                 |
| 5.6  | การใช้งาน ROM                     | firmware, โปรแกรมระบบ              |
| 5.7  | EPROM vs EEPROM vs Flash          | ต่างกันที่วิธีลบข้อมูล             |
| 5.8  | ขา (pin) DRAM                     | ใช้กำหนด address, data, control    |
| 5.9  | parity bit                        | bit ตรวจ error                     |
| 5.10 | syndrome (Hamming)                | ใช้บอกตำแหน่ง error                |
| 5.11 | SDRAM ต่างจาก DRAM                | ใช้ clock sync                     |
| 5.12 | DDR RAM คือ                       | ส่งข้อมูล 2 ครั้งต่อ clock         |
| 5.13 | NAND vs NOR                       | NAND เร็ว/หนาแน่น, NOR เข้าถึงสุ่ม |
| 5.14 | memory ใหม่ 3 แบบ                 | STT-RAM, PCRAM, ReRAM              |

🔹 🧮 Problems (สรุปแนวโจทย์)
| ข้อ       | แนวคิด                         |
| --------- | ------------------------------ |
| 5.1       | เปรียบ RAM vs ROM organization |
| 5.2       | คำนวณเวลา refresh              |
| 5.3       | คำนวณ memory cycle + data rate |
| 5.4       | ออกแบบ memory system           |
| 5.5       | วิเคราะห์ timing DRAM          |
| 5.6       | คำนวณ refresh rate             |
| 5.7       | วิเคราะห์ SRAM                 |
| 5.8       | ออกแบบ memory                  |
| 5.9       | คำนวณ MTBF                     |
| 5.10–5.14 | ใช้ Hamming code               |
