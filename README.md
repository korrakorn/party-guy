# 🍻 Party Tab Tracker

แอพเดียวไฟล์ `index.html` สำหรับเก็บข้อมูลค่าใช้จ่ายปาร์ตี้/เหล้ารายเดือน ตั้งลิมิตได้ มี chart แบบ "แก้วเหล้า" และล็อกอินด้วยอีเมล/รหัสผ่าน เก็บข้อมูลบน Supabase (ฟรี)

---

## 1) สร้างโปรเจกต์ Supabase (ฟรี)

1. ไปที่ [supabase.com](https://supabase.com) → กด **Start your project** → สมัคร/ล็อกอินด้วย GitHub
2. กด **New project**
   - ตั้งชื่อโปรเจกต์ เช่น `party-tab`
   - ตั้ง Database Password (เก็บไว้ดี ๆ ไม่ต้องใช้ในแอพแต่ใช้กรณีอื่น)
   - เลือก Region ที่ใกล้ที่สุด (เช่น Singapore)
   - กด **Create new project** รอประมาณ 1-2 นาที

## 2) สร้างตารางข้อมูล

1. ในเมนูซ้ายของ Supabase กด **SQL Editor**
2. กด **New query**
3. เปิดไฟล์ `supabase-setup.sql` ที่อยู่ในโฟลเดอร์นี้ คัดลอกทั้งหมด วางในช่อง SQL Editor
4. กด **Run** (หรือ Ctrl/Cmd + Enter)
5. ถ้าสำเร็จจะเห็น "Success. No rows returned" — ตอนนี้มี 2 ตารางคือ `expenses` และ `monthly_limits` พร้อม Row Level Security เปิดอยู่ (แต่ละคนเห็นแค่ข้อมูลตัวเอง)

## 3) เปิดระบบ Email/Password Login

ปกติ Supabase เปิด Email auth ไว้เป็น default อยู่แล้ว แต่ถ้าอยากปิดการยืนยันอีเมล (เพื่อทดสอบง่าย ๆ ไม่ต้องเช็คอีเมลจริง):

1. ไปที่ **Authentication** (ไอคอนรูปคน) ในเมนูซ้าย
2. กด **Providers** → **Email**
3. ปิด toggle **Confirm email** (ถ้าอยากให้สมัครแล้วใช้ได้ทันที ไม่ต้องกดยืนยันในอีเมล)
4. กด **Save**

> ถ้าเปิด Confirm email ไว้ ผู้ใช้ต้องเช็คอีเมลแล้วกดลิงก์ยืนยันก่อนจะล็อกอินได้

## 4) เอา API Key มาใส่ในแอพ

1. ไปที่ **Project Settings** (ไอคอนเฟือง) → **API**
2. คัดลอกค่า 2 ตัวนี้:
   - **Project URL** (เช่น `https://abcdefgh.supabase.co`)
   - **Project API keys → anon public** (key ยาว ๆ ขึ้นต้นด้วย `eyJ...`)
3. เปิดไฟล์ `index.html` หาบรรทัดนี้ใกล้ ๆ ด้านบนของ `<script>`:

```js
const SUPABASE_URL = "https://YOUR_PROJECT_ID.supabase.co";
const SUPABASE_ANON_KEY = "YOUR_PUBLIC_ANON_KEY";
```

4. แก้เป็นค่าจริงของคุณ แล้วเซฟไฟล์

> **หมายเหตุเรื่องความปลอดภัย**: `anon public` key ตัวนี้ถูกออกแบบมาให้เปิดเผยในโค้ด frontend ได้ ไม่ใช่ secret key — ความปลอดภัยของข้อมูลแต่ละคนถูกป้องกันด้วย Row Level Security (RLS) ที่ตั้งไว้ในขั้นตอนที่ 2 แล้ว อย่าใช้ `service_role` key ในไฟล์นี้เด็ดขาด

## 5) ทดสอบในเครื่องตัวเอง

เปิดไฟล์ `index.html` ด้วย browser ได้เลย (ดับเบิลคลิก หรือลาก drop ใส่ browser) ลองสมัครสมาชิกด้วยอีเมลใดก็ได้ แล้วเริ่มบันทึกรายจ่ายดูได้ทันที

## 6) เอาขึ้น GitHub + Host ฟรี

```bash
git init
git add .
git commit -m "Party tab tracker"
git branch -M main
git remote add origin https://github.com/<your-username>/party-tab.git
git push -u origin main
```

จากนั้น host ฟรีได้หลายทาง เช่น:

- **GitHub Pages**: ไปที่ repo → Settings → Pages → Branch เลือก `main` → Save รอ 1-2 นาที จะได้ลิงก์ `https://<username>.github.io/party-tab/`
- **Netlify / Vercel**: ลาก-วางโฟลเดอร์นี้เข้าไปที่ [netlify.com/drop](https://app.netlify.com/drop) ได้ลิงก์ทันที

---

## โครงสร้างไฟล์

```
party-tracker/
├── index.html          ← แอพทั้งหมด (HTML+CSS+JS ไฟล์เดียว)
├── supabase-setup.sql  ← รัน 1 ครั้งใน Supabase SQL Editor
└── README.md           ← ไฟล์นี้
```

## ฟีเจอร์

- ✅ ล็อกอิน/สมัครสมาชิกด้วยอีเมล+รหัสผ่าน (Supabase Auth)
- ✅ บันทึกรายจ่ายปาร์ตี้/เหล้า พร้อมโน้ตและวันที่
- ✅ ตั้งลิมิตงบประมาณแยกตามเดือน
- ✅ เลือกดูเดือนก่อนหน้าได้ (12 เดือนล่าสุด)
- ✅ Chart รูปแก้วเหล้าที่เติมตามเปอร์เซ็นต์การใช้เงิน เปลี่ยนสีเมื่อใกล้/เกินลิมิต
- ✅ ลบรายการได้
- ✅ ข้อมูลแต่ละคน private ด้วย Row Level Security — ใครก็เห็นแค่ของตัวเอง

## แก้ไข/ต่อยอดได้

- เปลี่ยนสีธีมได้ใน CSS variables ที่ด้านบนของ `<style>` (ตัวแปร `--orange`, `--mint`, `--amber` ฯลฯ)
- อยากเพิ่ม category (เบียร์/ค็อกเทล/ค่าทริป) → เพิ่มคอลัมน์ `category` ในตาราง `expenses` แล้วเพิ่ม dropdown ในฟอร์ม
- อยากมี export เป็น CSV → ใช้ข้อมูลใน `entriesCache` แปลงเป็น CSV ได้เลยฝั่ง JS
