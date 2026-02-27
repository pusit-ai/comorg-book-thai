# ภาคผนวก: การจัดการ Line Ending ด้วยไฟล์ .gitattributes

[← กลับไปสารบัญหลัก](README.md) | [ภาคผนวก: แก้ปัญหาที่พบบ่อย ←](appendix-troubleshooting.md)

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

[← กลับไปสารบัญหลัก](README.md) | [ภาคผนวก: แก้ปัญหาที่พบบ่อย ←](appendix-troubleshooting.md)
