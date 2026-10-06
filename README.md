[README.md](https://github.com/user-attachments/files/33096821/README.md)
# ระบบตรวจสอบสิทธิ์ตรวจสุขภาพประจำปี 2569

Google Apps Script Web App (อ่านข้อมูลจาก Google Sheets, สร้าง PDF ผ่าน Google Docs)

```
src/
  Code.js          # โค้ดฝั่งเซิร์ฟเวอร์ (Code.gs)
  index.html       # หน้าเว็บ
  appsscript.json  # manifest (timezone, Docs API, web app)
```

## ตั้งค่าครั้งแรก

1. `npm install -g @google/clasp@2.4.2` แล้ว `clasp login`
2. เปิด https://script.google.com/home/usersettings แล้วเปิด **Google Apps Script API**
3. คัดลอก `.clasp.json.example` เป็น `.clasp.json` แล้วใส่ Script ID
   (Apps Script > Project Settings > Script ID)
4. `clasp push --force` เพื่อส่งโค้ดขึ้น Apps Script ครั้งแรก
5. เปิดโปรเจกต์ใน editor แล้วกด Run ฟังก์ชัน `authorizeDriveAccess` 1 ครั้งเพื่ออนุญาตสิทธิ์
6. Deploy > New deployment > Web app (ครั้งเดียว) แล้วจด **Deployment ID**
   ปรับ `webapp.access` ใน `src/appsscript.json` ตามต้องการ (MYSELF / DOMAIN / ANYONE)

## Deploy อัตโนมัติด้วย GitHub Actions

ที่ repo: Settings > Secrets and variables > Actions เพิ่ม 3 secrets

| Secret | ค่า |
|---|---|
| `CLASPRC_JSON` | เนื้อหาไฟล์ `~/.clasprc.json` (เกิดหลัง `clasp login`) |
| `SCRIPT_ID` | Script ID |
| `DEPLOYMENT_ID` | Deployment ID จากขั้นตอนที่ 6 |

จากนั้นทุกครั้งที่ push เข้า `main` ระบบจะ push โค้ดและอัปเดตเวอร์ชัน deployment เดิมให้เอง

## ข้อควรระวัง

- **ใช้ GitHub Pages ไม่ได้** เพราะหน้าเว็บเรียก `google.script.run` ซึ่งทำงานได้เฉพาะบน Apps Script
- ข้อมูลเป็นข้อมูลสุขภาพพนักงาน (PDPA) ให้ตั้ง repo เป็น **Private** และห้าม commit `.clasprc.json`
- ควรย้าย `SPREADSHEET_ID` ไปไว้ใน Script Properties แทนการฝังในโค้ด
