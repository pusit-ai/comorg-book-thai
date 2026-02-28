# ภาคผนวก: การจัดการ Line Ending ด้วยไฟล์ .gitattributes

[← กลับไปสารบัญหลัก](README.md) | [ภาคผนวก: แก้ปัญหาที่พบบ่อย ←](appendix-troubleshooting.md)

---

**หมายเหตุ:** เอกสารนี้เป็นทางเลือก อ่านเมื่อพบ Warning เกี่ยวกับ `LF` หรือ `CRLF` ระหว่างการ commit (เช่น เมื่อสมาชิกใช้ Windows บางคนใช้ Mac)

---

## ปัญหาที่พบ

เมื่อ commit ไฟล์บน Windows อาจพบข้อความ:

```
warning: in the working copy of 'filename.md', 
LF will be replaced by CRLF the next time Git touches it
```

### ความหมาย

- **LF** (Line Feed) เป็นรูปแบบการขึ้นบรรทัดของ macOS และ Linux
- **CRLF** (Carriage Return + Line Feed) เป็นรูปแบบการขึ้นบรรทัดของ Windows

หากไม่กำหนดมาตรฐานเดียวกัน อาจทำให้เกิดการเปลี่ยนแปลงไฟล์โดยไม่จำเป็น และ Warning ซ้ำ ๆ

---

## แนวทางแก้ไข

สร้างไฟล์ชื่อ `.gitattributes` ไว้ที่โฟลเดอร์หลักของโปรเจกต์

### วิธีที่ 1: ใช้ Command Line

```bash
cd path/to/comorg-book-thai
echo * text=auto eol=lf > .gitattributes
git add .gitattributes
git commit -m "add .gitattributes to normalize line endings"
git push
```

### วิธีที่ 2: สร้างด้วย Text Editor

1. สร้างไฟล์ใหม่ชื่อ `.gitattributes` ในโฟลเดอร์หลัก
2. ใส่เนื้อหา:

```
* text=auto eol=lf
```

3. บันทึกไฟล์ แล้วสั่ง:

```bash
git add .
git commit -m "add .gitattributes"
git push
```

---

## ปรับไฟล์เก่าให้เป็นมาตรฐานเดียวกัน

หลังเพิ่ม `.gitattributes` แล้ว ควรปรับไฟล์เดิมทั้งหมด:

```bash
git add --renormalize .
git commit -m "normalize line endings"
git push
```

---

## ความหมายของบรรทัดกำหนดค่า

```
* text=auto eol=lf
```

| ส่วน | ความหมาย |
| :--- | :--- |
| `*` | ใช้กับทุกไฟล์ |
| `text=auto` | ให้ Git ตรวจสอบไฟล์ text โดยอัตโนมัติ |
| `eol=lf` | บังคับให้เก็บไฟล์ใน Repository เป็น LF เสมอ |

แม้สมาชิกใช้ Windows ระบบจะเก็บไฟล์ใน Git เป็น LF ทำให้ทำงานร่วมกับ macOS และ Linux ได้อย่างราบรื่น

---

## สรุป

การกำหนดมาตรฐาน Line Ending ช่วยให้:

- ลด Warning ที่ไม่จำเป็น
- ป้องกัน diff ผิดปกติ
- ทำงานข้ามระบบปฏิบัติการได้อย่างราบรื่น

---
# คู่มือ GitHub Desktop สำหรับนิสิตปี 1 (Step-by-Step)

GitHub Desktop คือโปรแกรมที่ช่วยให้ใช้งาน Git และ GitHub ได้ง่ายขึ้นโดยไม่ต้องใช้คำสั่งใน Terminal เหมาะสำหรับผู้เริ่มต้น เขียนโปรแกรมไม่เป็น หรือยังไม่ชำนาญ Git บนบรรทัดคำสั่ง

## 1. ดาวน์โหลดและติดตั้ง GitHub Desktop

1. เข้าเว็บไซต์ [https://desktop.github.com/](https://desktop.github.com/)
2. กดปุ่ม “Download for Windows” หรือ “Download for macOS”
3. ดับเบิลคลิกไฟล์เพื่อติดตั้งตามขั้นตอน

## 2. ตั้งค่าบัญชี GitHub

1. เปิดโปรแกรม GitHub Desktop
2. หากมีบัญชี GitHub แล้ว ให้ **Sign in with your browser**  
   ถ้ายังไม่มี ให้กด **Create account** (สมัครฟรี)
3. ยืนยันตัวตนตามคำแนะนำบนหน้าเว็บ

## 3. โคลน (Clone) โปรเจกต์ลงคอมพิวเตอร์

1. ให้หัวหน้ากลุ่มส่งลิงก์รีโปตัวอย่าง (repository) ให้ เช่น  
   `https://github.com/username/comorg-book-thai`
2. ที่โปรแกรม GitHub Desktop เลือกเมนู  
   **File > Clone repository...**
3. วางลิงก์ แล้วเลือกโฟลเดอร์ที่ต้องการเก็บไฟล์บนเครื่อง  
   กด **Clone**

## 4. ดูไฟล์และเริ่มทำงาน

- ไฟล์ทั้งหมดจาก GitHub จะอยู่ในโฟลเดอร์ที่เลือกไว้
- สามารถเปิดโฟลเดอร์ด้วย VS Code, Notepad, หรือโปรแกรมอื่น
- **แก้ไขไฟล์ตามหน้าที่** เช่น แก้เนื้อหา 4-1.md หรือ README.md

## 5. วิธีดึงงานล่าสุดก่อนเริ่มทำ

1. เปิด GitHub Desktop
2. ดูแถบเมนูบนสุด กดปุ่ม **Fetch origin**  
   เพื่อดึงงานอัปเดตล่าสุดจากกลุ่มมาที่เครื่องเรา ก่อนเริ่มทำเสมอ

## 6. วิธีส่งงานกลับขึ้น GitHub (Push)

1. หลังแก้ไขไฟล์เสร็จ ไฟล์ที่เปลี่ยนจะแสดงในช่อง **Changes**
2. เขียนข้อความสั้นๆในช่อง **Summary** เช่น  
   `เพิ่มเนื้อหาบทที่ 4-1`
3. กด **Commit to main** (หรือเป็นชื่อ branch อื่น)
4. กดปุ่ม **Push origin** (สีฟ้า) เพื่อส่งงานขึ้น GitHub

## 7. ตัวอย่างภาพหน้าจอ (ถ้ามี)

(สามารถแทรกรูปหน้าจอแต่ละขั้นตอน เพื่อให้นิสิตเห็นภาพจริง)

---

## ทริคและแนวทางปฏิบัติ

- **ดึงงาน (Fetch/ Pull)** ก่อนเริ่มทุกครั้ง ป้องกันงานทับกัน
- **Commit บ่อยๆ** เมื่อทำเสร็จแต่ละส่วน
- ห้ามแก้ไฟล์หรือโฟลเดอร์ของกลุ่มอื่นโดยไม่แจ้ง
- หากเกิด Conflict (ไฟล์ชนกัน) โปรแกรม GitHub Desktop จะมีคำแนะนำอัตโนมัติ

---

## คำถามที่พบบ่อย

**ถาม:** ถ้าทำไฟล์หายหรือผิดพลาดทำอย่างไร?  
**ตอบ:** สามารถดึงไฟล์จาก GitHub Desktop หรือ ขอดึงเวอร์ชันเก่าคืนได้

**ถาม:** ใช้ GitHub Desktop กับ VS Code ได้ไหม?  
**ตอบ:** ได้ ทุกครั้งที่ต้องการแก้ไฟล์ ให้เปิด VS Code จากเมนู “Open in Visual Studio Code” ใน GitHub Desktop ได้เลย

---

## สรุปสำหรับนิสิตปี 1

- GitHub Desktop เหมาะกับผู้เริ่มต้น ไม่ต้องท่อง git command
- ลากไฟล์ ใส่โน้ต แล้วกดส่งงาน (push) ได้ง่ายๆ
- ทำซ้ำเป็นประจำ: ดึงงาน → แก้งาน → Commit → Push

หากมีข้อสงสัยให้สอบถามหัวหน้ากลุ่มหรือรุ่นพี่ได้ทันที!

---
[← กลับไปสารบัญหลัก](README.md) | [ภาคผนวก: แก้ปัญหาที่พบบ่อย ←](appendix-troubleshooting.md)
