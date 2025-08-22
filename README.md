# MyCI/CD - ระบบจัดการผู้ใช้และการปรับใช้อัตโนมัติ

## ภาพรวมของโปรเจ็ค

MyCI/CD เป็นโปรเจ็คเว็บแอปพลิเคชันขนาดเล็กที่แสดงให้เห็นถึงการใช้งาน GitHub Actions สำหรับ CI/CD (Continuous Integration/Continuous Deployment) ร่วมกับ GitHub Pages เพื่อการปรับใช้อัตโนมัติ

### ฟีเจอร์หลัก
- 🏠 **หน้าแรก (Index Page)**: หน้าต้อนรับผู้ใช้
- 📝 **ระบบสมัครสมาชิก**: ฟอร์มสมัครสมาชิกที่ครอบคลุมพร้อมการตรวจสอบความถูกต้อง
- 🚀 **การปรับใช้อัตโนมัติ**: ใช้ GitHub Actions เพื่อปรับใช้ไปยัง GitHub Pages
- 📱 **Responsive Design**: ออกแบบให้รองรับการใช้งานบนอุปกรณ์ต่างๆ

## โครงสร้างไฟล์

```
mycicd/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions workflow สำหรับการปรับใช้
├── .gitignore                  # ไฟล์ที่ Git จะไม่ติดตาม
├── README.md                   # เอกสารนี้
├── index.html                  # หน้าแรกของเว็บไซต์
└── register.html               # หน้าสมัครสมาชิก
```

## คำอธิบายฟังก์ชันการทำงาน

### 1. หน้าแรก (index.html)
- เป็นหน้าต้อนรับผู้ใช้แบบเรียบง่าย
- แสดงข้อความต้อนรับ "Welcome to the Index Page"

### 2. ระบบสมัครสมาชิก (register.html)
ฟอร์มสมัครสมาชิกที่มีฟิลด์ดังนี้:

#### ฟิลด์ที่จำเป็น (Required):
- **ชื่อผู้ใช้ (Username)**: ความยาว 3-20 ตัวอักษร
- **อีเมล (Email)**: ต้องเป็นรูปแบบอีเมลที่ถูกต้อง
- **เพศ (Gender)**: เลือกจาก ชาย/หญิง/อื่นๆ
- **บทบาท (Role)**: เลือกจาก Administrator, User, Moderator, Editor, Viewer, Guest

#### ฟิลด์เสริม:
- **ประเทศ (Country)**: เลือกจากรายการประเทศต่างๆ

#### ฟีเจอร์พิเศษ:
- ✅ การตรวจสอบความถูกต้องแบบ Real-time
- 🎨 ออกแบบ UI ที่สวยงามและใช้งานง่าย
- 📤 การแสดงข้อมูลที่กรอกผ่าน Alert และ Console
- 🔙 ลิงก์กลับไปหน้าแรก

## GitHub Actions Workflow

### ไฟล์: `.github/workflows/deploy.yml`

เป็น workflow สำหรับการปรับใช้อัตโนมัติที่มีคุณสมบัติดังนี้:

### เงื่อนไขการเริ่มทำงาน (Triggers):
```yaml
on:
  push:
    tags: ['*']           # เมื่อมีการ push tag ใดๆ
    branches: [main]      # เมื่อมีการ push ไปยัง branch main
  workflow_dispatch:      # การเรียกใช้แบบ manual
```

### การทำงานของ Workflow:

1. **Checkout Repository**: ดาวน์โหลดซอร์สโค้ดล่าสุด
2. **Setup Pages**: ตั้งค่า GitHub Pages
3. **Prepare Deployment Files**: 
   - สร้างโฟลเดอร์ `_site`
   - คัดลอกไฟล์ HTML ทั้งหมดไปยัง `_site`
4. **Upload Artifact**: อัพโหลดไฟล์เพื่อการปรับใช้
5. **Deploy to GitHub Pages**: ปรับใช้ไปยัง GitHub Pages

### การรักษาความปลอดภัย:
- ใช้ `concurrency` เพื่อป้องกันการปรับใช้พร้อมกัน
- กำหนด `permissions` ที่เหมาะสม:
  - `contents: read` - อ่านเนื้อหา repository
  - `pages: write` - เขียนไปยัง GitHub Pages
  - `id-token: write` - สร้าง ID token

## วิธีการใช้งาน

### 1. การเข้าถึงเว็บไซต์
เมื่อ workflow ทำงานสำเร็จ เว็บไซต์จะพร้อมใช้งานที่:
```
https://[ชื่อผู้ใช้].github.io/mycicd
```

### 2. การใช้งานระบบสมัครสมาชิก
1. เข้าไปที่ `register.html`
2. กรอกข้อมูลในฟอร์ม
3. กดปุ่ม "Register"
4. ระบบจะแสดงข้อมูลที่กรอกผ่าน Alert

### 3. การพัฒนาต่อ (Development)

#### การเปลี่ยนแปลงโค้ด:
1. แก้ไขไฟล์ HTML ตามต้องการ
2. Commit และ Push ไปยัง branch `main`
3. Workflow จะทำงานอัตโนมัติและปรับใช้การเปลี่ยนแปลง

#### การทดสอบ Local:
```bash
# เปิดไฟล์ HTML ด้วย web browser
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

## การกำหนดค่าเพิ่มเติม

### การแก้ไข Workflow
หากต้องการปรับแต่ง workflow สามารถแก้ไขไฟล์ `.github/workflows/deploy.yml`:

```yaml
# ตัวอย่างการเพิ่ม step ใหม่
- name: Custom Step
  run: |
    echo "Your custom commands here"
```

### การเพิ่มไฟล์ใหม่
1. เพิ่มไฟล์ HTML, CSS, หรือ JavaScript ใหม่
2. แก้ไข workflow หากจำเป็น (ในส่วน `cp *.html _site/`)
3. Commit และ Push

## การแก้ไขปัญหา

### ปัญหาที่อาจพบ:
1. **Workflow ไม่ทำงาน**: ตรวจสอบ permissions ใน repository settings
2. **หน้าเว็บไม่แสดง**: รอสักครู่หลัง deploy เสร็จ (อาจใช้เวลา 5-10 นาที)
3. **ไฟล์ไม่อัพเดต**: ตรวจสอบว่า workflow run สำเร็จ

### การตรวจสอบ Workflow:
1. ไปที่ tab "Actions" ใน GitHub repository
2. คลิกที่ workflow run ล่าสุด
3. ตรวจสอบ log ของแต่ละ step

## เทคโนโลยีที่ใช้

- **HTML5**: โครงสร้างหน้าเว็บ
- **CSS3**: การจัดรูปแบบและ responsive design
- **JavaScript**: การควบคุมฟอร์มและ validation
- **GitHub Actions**: CI/CD pipeline
- **GitHub Pages**: การ hosting

## การสนับสนุนและการพัฒนา

### การ Contribute:
1. Fork repository นี้
2. สร้าง feature branch ใหม่
3. ทำการเปลี่ยนแปลง
4. ส่ง Pull Request

### ข้อเสนอแนะ:
หากมีข้อเสนอแนะหรือพบปัญหา กรุณาสร้าง Issue ใน GitHub repository

---

## License
โปรเจ็คนี้เป็นโอเพนซอร์สและใช้เพื่อการศึกษา

**สร้างโดย**: NPKMUTNB  
**อัพเดตล่าสุด**: 2024