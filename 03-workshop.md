# ส่วนที่ 3: บททดสอบก่อนลงสนามจริง (มินิเวิร์กชอป 15 นาที)

[← กลับไปสารบัญหลัก](README.md) | [ก่อนหน้า: การตั้งค่า Git](02-git-setup.md) | [ถัดไป: การทำงานโครงงานจริง →](04-project-workflow.md)

---

## จุดประสงค์

เพื่อให้นิสิตฝึกฝนการทำงานร่วมกันผ่าน Git และ GitHub ก่อนลงมือทำโครงงานจริง เพื่อป้องกันความผิดพลาดและสร้างความเข้าใจในขั้นตอนการทำงาน

**กติกาสำคัญ:** ห้ามแก้ไขไฟล์เดียวกัน ให้สร้างไฟล์เป็นชื่อเล่นของตนเองเท่านั้น

---

## หน้าที่ของหัวหน้ากลุ่ม

1. เข้าเว็บ [GitHub](https://github.com) กด **New** สร้าง Repository ใหม่ชื่อ `git-workshop-101` (ไม่ต้องติ๊ก "Add a README file")
2. ไปที่ **Settings** > **Collaborators** ใส่อีเมล GitHub ของสมาชิกในกลุ่มแล้วส่งคำเชิญ
3. เปิด Command Prompt หรือ Terminal สร้างโครงสร้างเริ่มต้นและส่งขึ้นเว็บดังนี้:

```bash
git clone https://github.com/ชื่อหัวหน้า/git-workshop-101.git
cd git-workshop-101
mkdir members
git add .
git commit -m "หัวหน้าสร้างโฟลเดอร์ members"
git push origin main
```

**ผลลัพธ์ที่พึงได้:**
- บน GitHub: หน้า Repository จะเห็นโฟลเดอร์ `members` และไฟล์ภายใน
- ในเครื่อง: โครงสร้างโฟลเดอร์จะเป็น `git-workshop-101/members/`

---

## หน้าที่ของลูกทีมทุกคน (รวมหัวหน้าด้วย)

### ขั้นที่ 1: ยอมรับคำเชิญและดึงโปรเจกต์ลงมา

กดยอมรับคำเชิญในอีเมล แล้วพิมพ์คำสั่ง:

```bash
git clone https://github.com/ชื่อหัวหน้า/git-workshop-101.git
cd git-workshop-101
```

### ขั้นที่ 2: สร้างสาขา (Branch) ส่วนตัว

ตั้งชื่อสาขาเป็นชื่อเล่นของตนเอง (ภาษาอังกฤษ):

```bash
git checkout -b name-somchai
```

### ขั้นที่ 3: สร้างไฟล์และเขียนข้อความ

เข้าไปในโฟลเดอร์ `members` สร้างไฟล์ใหม่ชื่อ `somchai.md` (ใช้ชื่อตนเอง) พิมพ์ข้อความทักทาย แล้วบันทึกไฟล์

### ขั้นที่ 4: ส่งงานขึ้นเว็บ

```bash
git add .
git commit -m "ส่งป้ายชื่อของสมชาย"
git push -u origin name-somchai
```

**ผลลัพธ์ที่พึงได้ (ตัวอย่าง):**

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 285 bytes | 285.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: ...
To https://github.com/ชื่อหัวหน้า/git-workshop-101.git
 * [new branch]      name-somchai -> name-somchai
```

จากนั้นบน GitHub จะแสดงข้อความสีเหลืองว่า "name-somchai had recent pushes" พร้อมปุ่ม **Compare & pull request**

---

## การตรวจรับงาน (ทำบนเว็บ GitHub)

1. **ลูกทีม:** เข้า Repository ของกลุ่ม กดปุ่ม **Compare & pull request** จากนั้นกด **Create pull request**
2. **หัวหน้ากลุ่ม:** ไปที่แท็บ **Pull requests** ตรวจสอบงาน แล้วกด **Merge pull request**
3. **ทุกคน:** ดึงข้อมูลที่รวมแล้วลงเครื่อง:

```bash
git checkout main
git pull origin main
```

**ผลลัพธ์ที่พึงได้:**

เมื่อเปิดโฟลเดอร์ `members` ดู โครงสร้างจะประมาณนี้:

```
git-workshop-101/
└── members/
    ├── somchai.md      ← ไฟล์ของสมชาย
    ├── wichai.md       ← ไฟล์ของวิชัย
    └── srisuda.md      ← ไฟล์ของศรีสุดา
```

ไฟล์ของสมาชิกทุกคนมารวมกันในโฟลเดอร์เดียวกัน แสดงว่าทำงานร่วมกันสำเร็จ

---

[← กลับไปสารบัญหลัก](README.md) | [ก่อนหน้า: การตั้งค่า Git](02-git-setup.md) | [ถัดไป: การทำงานโครงงานจริง →](04-project-workflow.md)
