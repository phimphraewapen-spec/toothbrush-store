# SmileBrush — Toothbrush Store

โปรเจกต์ Landing page ร้านแปรงสีฟันตัวอย่าง ใช้ React + Vite พร้อมตะกร้าสินค้าและ Checkout

## สินค้าตัวอย่าง
- `TB-001` Soft Clean — 89 บาท
- `TB-002` Charcoal Clean — 129 บาท

แก้ชื่อ ราคา รายละเอียด และ SKU ได้ใน `src/App.jsx` ที่ตัวแปร `products`

## 1) เปิดโปรเจกต์ในเครื่อง
ต้องติดตั้ง Node.js เวอร์ชัน 18 ขึ้นไป

```bash
npm install
npm run dev
```

เปิด URL ที่ Vite แสดง เช่น `http://localhost:5173`

## 2) เชื่อม Google Sheets
1. เปิด Google Sheets template ที่เตรียมไว้ แล้วสร้าง/ตรวจสอบชีต `Orders` และ `Order_Items`
2. ใน Google Sheets เลือก Extensions → Apps Script
3. คัดลอกโค้ดจาก `apps-script/Code.gs` ไปวางใน Apps Script
4. Deploy → New deployment → Web app
5. Execute as: Me; Who has access: Anyone
6. คัดลอก Web app URL
7. สร้างไฟล์ `.env` ที่ root โดยใส่:

```env
VITE_GOOGLE_SCRIPT_URL=https://script.google.com/macros/s/ใส่_DEPLOYMENT_ID/exec
```

8. เมื่อตั้งค่าแล้วให้ build/deploy ใหม่ คำสั่งซื้อจะถูกส่งเข้า Google Sheets
9. ถ้ายังไม่ตั้งค่า URL เว็บไซต์จะทำงานใน Demo mode และเก็บคำสั่งซื้อไว้ใน localStorage ของ browser เท่านั้น

### โครงสร้างชีตที่ Apps Script ใช้
`Orders` columns:
Order ID, Created At, Customer Name, Phone, Email, Address, Province, Postcode, Note, Total, Status

`Order_Items` columns:
Order ID, SKU, Product Name, Quantity, Unit Price, Line Total

> หมายเหตุ: Apps Script ตัวอย่างนี้เป็น receiver สำหรับบันทึกออเดอร์ ไม่ใช่ระบบชำระเงิน และยังไม่มีการเชื่อม API ขนส่ง

## 3) อัปโหลดขึ้น GitHub
สร้าง repository ชื่อ `toothbrush-store` ที่บัญชี `phimphraewapen-spec` แล้วอัปโหลดไฟล์ทั้งหมดในโฟลเดอร์นี้ (อย่าอัปโหลดไฟล์ `.env`)

ถ้าใช้ Git ในเครื่อง:

```bash
git init
git add .
git commit -m "Initial SmileBrush storefront"
git branch -M main
git remote add origin https://github.com/phimphraewapen-spec/toothbrush-store.git
git push -u origin main
```

## 4) Deploy บน Vercel
1. เข้า https://vercel.com และ Sign up / Log in ด้วย GitHub
2. กด Add New → Project
3. Import repository `phimphraewapen-spec/toothbrush-store`
4. Framework preset: Vite (ปกติ Vercel ตรวจจับให้อัตโนมัติ)
5. Build command: `npm run build`
6. Output directory: `dist`
7. เพิ่ม Environment Variable:
   - Name: `VITE_GOOGLE_SCRIPT_URL`
   - Value: URL ของ Apps Script Web App
8. กด Deploy

หลังจาก push commit ใหม่เข้า GitHub, Vercel จะ build และ deploy ให้อัตโนมัติเมื่อเชื่อม repository แล้ว

## โครงสร้างโปรเจกต์
```text
toothbrush-store/
├── apps-script/Code.gs
├── public/images/TB-001.svg
├── public/images/TB-002.svg
├── src/lib/api.js
├── src/App.jsx
├── src/main.jsx
├── src/styles.css
├── .env.example
├── index.html
├── package.json
├── vercel.json
└── README.md
```

## ข้อควรทราบก่อนใช้งานจริง
- สินค้า ราคา และภาพเป็นตัวอย่าง สามารถเปลี่ยนได้
- ยังไม่มีระบบตัดสต็อกอัตโนมัติ, ระบบชำระเงิน, บัญชีผู้ดูแล หรือ API สร้างใบปะหน้าขนส่งจริง
- อย่าใส่ secret key หรือข้อมูลลับลง GitHub
- ควรทดสอบการส่งคำสั่งซื้อและการบันทึกลงชีตก่อนเปิดขายจริง
