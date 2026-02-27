# ภาคผนวก: กรณีศึกษาปัญหาที่พบบ่อย

[← กลับไปสารบัญหลัก](README.md) | [ภาคผนวก: การจัดการ Line Ending →](appendix-gitattributes.md)

---

เอกสารฉบับนี้อธิบายกรณีศึกษาปัญหาที่เกิดขึ้นจริงระหว่างการใช้งาน Git และ GitHub พร้อมแนวทางแก้ไขที่ถูกต้อง

---

## กรณีที่ 1: Push ไม่ได้ (Permission Denied)

### อาการ

เมื่อพิมพ์ `git push` ระบบแสดงข้อความลักษณะ:

```
remote: Permission denied
fatal: unable to access ...
```

### สาเหตุ

เครื่องกำลังยืนยันตัวตนด้วยบัญชี GitHub ที่ไม่มีสิทธิ์เขียนใน Repository นั้น

### แนวทางแก้ไข

- ตรวจสอบว่าใช้อีเมลและบัญชีที่ได้รับเชิญเข้าร่วม Repository แล้ว
- หากมีหลายบัญชี GitHub แนะนำให้ใช้ SSH Key แยกตามบัญชี (รายละเอียดเพิ่มเติมดูใน [เอกสาร GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh))

---

## กรณีที่ 2: Repository not found

### อาการ

```
ERROR: Repository not found.
```

### วิธีตรวจสอบ

```bash
git remote -v
```

### แนวทางแก้ไข

ตั้งค่า URL ของ remote ให้ถูกต้อง:

```bash
git remote set-url origin https://github.com/username/repository-name.git
```

---

## กรณีที่ 3: Push ถูกปฏิเสธ (fetch first)

### อาการ

```
! [rejected] main -> main (fetch first)
```

### ความหมาย

บน GitHub มี commit ใหม่ที่เครื่องเรายังไม่มี Git จึงป้องกันไม่ให้ทับงานผู้อื่น

### แนวทางแก้ไข

```bash
git pull --rebase origin main
git push
```

**ผลลัพธ์ที่พึงได้เมื่อแก้ไขสำเร็จ (ตัวอย่าง):**

```
Successfully rebased and updated refs/heads/main.
...
To https://github.com/.../comorg-book-thai.git
   abc1234..def5678  main -> main
```

---

## กรณีที่ 4: เกิด Conflict (ข้อขัดแย้ง)

### อาการ

เมื่อ merge หรือ pull มีข้อความแจ้ง conflict และไฟล์จะมีเครื่องหมายดังนี้:

```
<<<<<<< HEAD
เนื้อหาของเรา
=======
เนื้อหาจาก branch อื่น
>>>>>>> branch-name
```

### แนวทางแก้ไข

1. เปิดไฟล์ที่มี conflict
2. อ่านเนื้อหาทั้งสองฝั่ง (ระหว่าง `<<<<<<<` กับ `=======` และระหว่าง `=======` กับ `>>>>>>>`)
3. เลือกหรือรวมเนื้อหาให้ถูกต้อง
4. ลบเครื่องหมาย conflict marker (`<<<<<<<`, `=======`, `>>>>>>>`) ออกทั้งหมด
5. บันทึกไฟล์ แล้วพิมพ์:

```bash
git add .
git rebase --continue
```

**ผลลัพธ์ที่พึงได้เมื่อแก้ conflict สำเร็จ:** ระบบจะแสดง `Successfully rebased and updated refs/heads/main` จากนั้นให้สั่ง `git push` เพื่อส่งงานขึ้นเว็บ

---

## กรณีที่ 5: ทำงานหลายเครื่อง

### หลักการสำคัญ

| เวลา | การกระทำ |
| :--- | :--- |
| ก่อนเริ่มงาน | `git pull` |
| หลังทำงานเสร็จ | `git push` |

**ห้ามเริ่มพิมพ์งานทันทีโดยไม่ดึงงานล่าสุดก่อน**

---

## แนวคิดสำคัญที่ต้องเข้าใจ

ปัญหา Git ส่วนใหญ่เกี่ยวข้องกับ 3 ประเด็น:

1. **Authentication** — ใครคือคนที่กำลัง push อยู่
2. **Authorization** — บัญชีนั้นมีสิทธิ์ใน Repository หรือไม่
3. **Synchronization** — ประวัติในเครื่องกับบน GitHub ตรงกันหรือไม่

---

## Checklist ก่อนสั่ง git push

- [ ] ตรวจสอบ remote ด้วย `git remote -v`
- [ ] ดึงงานล่าสุดด้วย `git pull`
- [ ] ตรวจสอบ branch ด้วย `git branch`

---

[← กลับไปสารบัญหลัก](README.md) | [ภาคผนวก: การจัดการ Line Ending →](appendix-gitattributes.md)
