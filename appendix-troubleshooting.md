# ภาคผนวก: กรณีศึกษาปัญหาที่พบบ่อย

[← กลับไปสารบัญหลัก](README.md) | [ภาคผนวก: การจัดการ Line Ending →](appendix-gitattributes.md)

---

เอกสารฉบับนี้อธิบายกรณีศึกษาปัญหาที่เกิดขึ้นจริงระหว่างการใช้งาน Git และ GitHub พร้อมแนวทางแก้ไขที่ถูกต้อง โดยเรียงลำดับจากปัญหาพื้นฐานไปสู่ปัญหาที่ซับซ้อนมากขึ้น

---

# ภาพรวมโครงสร้างปัญหา Git

ปัญหา Git ส่วนใหญ่เกี่ยวข้องกับ 3 ประเด็นหลัก:

1. **Authentication** — ใครคือคนที่กำลัง push อยู่ (ยืนยันตัวตน)
2. **Authorization** — บัญชีนั้นมีสิทธิ์ใน Repository หรือไม่
3. **Synchronization** — ประวัติในเครื่องกับบน GitHub ตรงกันหรือไม่

---

## กรณีที่ 1: Push ไม่ได้ (Permission Denied)

### อาการ

เมื่อพิมพ์ `git push` ระบบแสดงข้อความลักษณะ:

```
remote: Permission denied
fatal: unable to access ...
```

หรือในกรณีใช้ SSH:

```
git@github.com: Permission denied (publickey).
```

### สาเหตุ

- ใช้บัญชีที่ไม่มีสิทธิ์เขียนใน Repository
- SSH key ยังไม่ได้เพิ่มเข้า GitHub
- ใช้หลายบัญชี GitHub แต่ไม่ได้แยก SSH key

### วิธีตรวจสอบ

```bash
ssh -T git@github.com
```

### แนวทางแก้ไข

- ตรวจสอบว่าได้รับเชิญเข้าร่วม Repository แล้ว
- เพิ่ม public key (`.pub`) เข้า GitHub
- หากมีหลายบัญชี แนะนำให้ใช้ SSH Key แยก และตั้งค่า `~/.ssh/config` ให้ถูกต้อง

ตัวอย่าง config:

```ssh
Host github-account-a
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_account_a
    IdentitiesOnly yes
```

---

## กรณีที่ 2: Repository not found

### อาการ

```
ERROR: Repository not found.
```

### สาเหตุ

- URL ผิด
- ชื่อ repository ผิด
- บัญชีไม่มีสิทธิ์เข้าถึง

### วิธีตรวจสอบ

```bash
git remote -v
```

### แนวทางแก้ไข

```bash
git remote set-url origin https://github.com/username/repository-name.git
```

หรือถ้าใช้ SSH alias:

```bash
git remote set-url origin git@github-account-a:username/repository-name.git
```

---

## กรณีที่ 3: Push ถูกปฏิเสธ (fetch first)

### อาการ

```
! [rejected] main -> main (fetch first)
```

### ความหมาย

บน GitHub มี commit ใหม่ที่เครื่องเรายังไม่มี  
Git ป้องกันไม่ให้ทับงานผู้อื่น

### แนวทางแก้ไข

```bash
git pull --rebase origin main
git push
```

---

## กรณีที่ 4: เกิด Conflict (ข้อขัดแย้ง)  
(เช่น กรณีแก้ไฟล์ global พร้อมกันหลายคน หรือไฟล์ที่กระทบต่อส่วนกลางของโปรเจกต์)

### อาการ

เมื่อทำงานร่วมกันในโครงการ software จริง เช่น ลูกทีมแต่ละคน clone โปรเจกต์ แล้วสร้าง branch ของตนเองไปพัฒนาฟีเจอร์ หรือแก้ไขงานที่ได้รับมอบหมาย แต่ถ้าหลายคนเผลอไปแก้ “ไฟล์ global” หรือไฟล์สำคัญที่กระทบทุกคน (เช่น config, core function, หรือไฟล์ statement ส่วนกลาง) พร้อมกัน เมื่อ merge หรือ pull งานกลับมา Git จะขึ้น Conflict และแสดงเครื่องหมายในไฟล์:

```
<<<<<<< HEAD
เนื้อหาของเรา
=======
เนื้อหาจาก branch อื่น
>>>>>>> branch-name
```

### ตัวอย่างสถานการณ์

- มีไฟล์ `config.py` หรือ `app.js` ที่ใช้ร่วมกันทั้งโปรเจกต์  
- ลูกทีม A กับ B ต่าง branch ไปทำงานของตัวเอง แต่ไปแก้ไขไฟล์นี้พร้อมกัน  
- เมื่อนำงานมา merge รวมกัน จะเกิด conflict

### แนวทางแก้ไข

1. เปิดไฟล์ที่มี conflict จะพบว่ามีเครื่องหมาย `<<<<<<<`, `=======`, `>>>>>>>` คั่นเนื้อหาสองชุด (จากฝั่งตนเองและอีก branch)
2. อ่านและพิจารณาว่าส่วนไหนควรเก็บ ส่วนไหนควรถูกแทนที่ หรือควรรวมเนื้อหาทั้งสองฝั่งให้เหมาะสม (ถ้าไม่มั่นใจควรคุยกับสมาชิกผู้ที่แก้ไขไฟล์เดียวกัน)
3. ลบเครื่องหมาย `<<<<<<<`, `=======`, `>>>>>>>` ออก ให้เหลือเฉพาะเนื้อหาที่ถูกต้อง
4. บันทึกไฟล์ แล้วพิมพ์:

```bash
git add FILE_NAME
git rebase --continue
```
*หรือถ้าใช้ merge จะใช้*
```bash
git commit
```
5. จากนั้นจึงค่อย push งานกลับขึ้น GitHub:

```bash
git push
```

#### **ข้อแนะนำเพิ่มเติม**
- หากต้องแก้ไขไฟล์ global หรือไฟล์สำคัญ ควรแจ้งสมาชิกในทีม หรือแบ่งเวลาการแก้ไขให้ไม่ชนกัน
- พยายามแยกงานแต่ละคนให้อยู่ในไฟล์ย่อยของตนเอง หรือ module/ฟังก์ชั่นเฉพาะ จะลดปัญหา conflict
- ถ้า conflict เยอะหรือเกิดกับไฟล์ที่อ่อนไหว ควรพูดคุยกันก่อน merge ทุกครั้ง


---

## กรณีที่ 5: not a git repository

### อาการ

```
fatal: not a git repository (or any of the parent directories): .git
```

### สาเหตุ

กำลังสั่งงาน Git ในโฟลเดอร์ที่ยังไม่ใช่ Git Repository

### วิธีตรวจสอบ

```bash
git status
```

### แนวทางแก้ไข

ถ้ายังไม่เคย clone:

```bash
git clone <repository-url>
```

หรือเข้าโฟลเดอร์ที่ถูกต้องก่อน:

```bash
cd repository-name
```

---

## กรณีที่ 6: ใช้หลาย SSH Key แล้ว Push ไม่ได้

### อาการ

```
git@github.com: Permission denied (publickey).
```

### สาเหตุ

- ใช้หลาย SSH key
- Clone ด้วย `github.com` ตรง ๆ
- ไม่ได้ใช้ alias ใน `~/.ssh/config`

### แนวทางแก้ไข

1. เพิ่ม public key เข้า GitHub
2. ตั้งค่า alias ใน `~/.ssh/config`
3. clone ด้วย alias เช่น:

```bash
git clone git@github-account-a:username/repository.git
```

4. ทดสอบก่อนใช้งาน:

```bash
ssh -T git@github-account-a
```

---

## กรณีที่ 7: ทำงานหลายเครื่อง

### หลักการสำคัญ

| เวลา | การกระทำ |
| :--- | :--- |
| ก่อนเริ่มงาน | `git pull` |
| หลังทำงานเสร็จ | `git push` |

**ห้ามเริ่มทำงานโดยไม่ดึงงานล่าสุดก่อน**

---

# Checklist ก่อนสั่ง git push

- [ ] ตรวจสอบ remote ด้วย `git remote -v`
- [ ] ตรวจสอบ branch ด้วย `git branch`
- [ ] ดึงงานล่าสุดด้วย `git pull`
- [ ] ตรวจสอบว่า `ssh -T` ผ่าน (กรณีใช้ SSH)
- [ ] ห้ามใช้ `git push -f` หากไม่จำเป็น

---

# สรุปแนวคิดสำคัญ

Git ถูกออกแบบมาเพื่อป้องกันข้อมูลสูญหาย  
ปัญหาส่วนใหญ่เกิดจาก:

- ใช้บัญชีผิด
- ใช้ URL ผิด
- ลืม pull ก่อน push
- ตั้งค่า SSH ไม่ถูกต้อง

หากเข้าใจโครงสร้าง  
**Working Directory → Commit → Remote**  
จะสามารถแก้ปัญหาได้อย่างเป็นระบบ

---

[← กลับไปสารบัญหลัก](README.md) | [ภาคผนวก: การจัดการ Line Ending →](appendix-gitattributes.md)