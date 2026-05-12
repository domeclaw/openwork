---
name: tim-tvpool-1
description: Entertainment news research skill — monitor Facebook/Twitter accounts provided by user, analyze engagement, and compile trending stories into workspace markdown reports.
command: /tim-tvpool-1
verified: true
---

# Tim TVPool #1 — Entertainment News Research

## ⚠️ เมื่อเรียก skill นี้

**เริ่มทำงานทันที — ห้ามถาม ห้ามขอ confirmation**

1. เปิด **internal browser** ทันที (`openwork-browser_show_browser`)
2. ดึง URLs จากข้อความที่ user ส่งมา — ถ้ามี URLs ใช้เลย ไม่ถาม
3. ถ้าไม่มี URLs → ถามแค่ครั้งเดียว: "ขอ URLs ของเพจ Facebook หรือ Twitter/X ที่ต้องการสรุปข่าวหน่อยครับ"
4. ได้ URLs แล้ว → scan 5-10 posts ล่าสุด → วิเคราะห์ engagement → เขียน report
5. ตอบโต้เป็นภาษาไทย

---

## Quick Usage

```
1. เปิด internal browser (openwork-browser_show_browser)
2. ไปทีละ URL → screenshot → scan 5-10 posts ล่าสุด
3. จด engagement: likes, comments, shares
4. ดูรีวิวที่มี engagement สูง → เปิดลิงก์ในคอมเมนต์
5. สร้างโฟลเดอร์ tvpool/[YYYY-MM-DD_HH-mm]/
6. เขียน report.md + save screenshots
```

## Language

**ตอบ user เป็นภาษาไทยเสมอ** — เนื้อหาโพสต์ต้นฉบับคงไว้ตามเดิม

---

## Browser

ใช้ **internal browser เท่านั้น** — ไม่ต้องถาม user

- เปิดด้วย `openwork-browser_show_browser`
- ถ้า Facebook/Twitter ต้อง login → บอก user ให้ login ใน browser แล้วทำต่อ

---

## Target Accounts

รับจาก user โดยตรง — **ไม่มี default accounts**

- ถ้า user ส่ง URLs มาพร้อมคำสั่ง → ใช้ URLs นั้นเลย ไม่ถามยืนยัน
- ถ้าไม่มี URLs ในข้อความ → ถามแค่ครั้งเดียว: "ขอ URLs ของเพจ Facebook หรือ Twitter/X ที่ต้องการสรุปข่าวหน่อยครับ"

---

## Scan Workflow

### Facebook (แต่ละเพจ):
```
1. ไปที่เพจ → screenshot
2. อ่าน 5-10 posts ล่าสุด (บน→ล่าง)
3. จด: เนื้อหา, likes, comments, shares, timestamp, hashtags
4. โพสต์ engagement สูง → ดูรีวิว ลิงก์ในคอมเมนต์
```

### Twitter/X (แต่ละ account):
```
1. ไปที่ profile → screenshot
2. อ่าน 5-10 tweets ล่าสุด
3. จด: เนื้อหา, likes, retweets, replies, views, hashtags
```

### Engagement Thresholds:
| ระดับ | Facebook | Twitter/X |
|-------|----------|-----------|
| HIGH | 100+ likes, 50+ comments | 100+ likes, 50+ retweets |
| MEDIUM | 20-99 likes, 10-49 comments | 20-99 likes, 10-49 retweets |
| LOW | <20 likes, <10 comments | <20 likes, <10 retweets |

---

## Folder Structure

```
tvpool/
  └── 2025-01-15_14-30/
      ├── report.md
      ├── screenshots/
      │   ├── fb-page1-post1.png
      │   └── twitter-account1-tweet1.png
      └── linked-content/
          └── article1-summary.md
```

---

## Report Template

```markdown
# Tim TVPool Report — [YYYY-MM-DD HH:mm]

## สรุปข่าวเด่นวันนี้

[2-3 บรรทัดสรุป]

---

## 🔥 ข่าวมาแรง (HIGH Engagement)

### [หัวข้อ]
- **แหล่ง**: [เพจ/account]
- **Engagement**: X likes, Y comments, Z shares
- **เนื้อหา**: [สรุปสั้นๆ]
- **Sentiment**: บวก/ลบ/กลาง
- **Hashtags**: #...
- **ลิงก์ที่เกี่ยวข้อง**: [ชื่อ](URL) — สรุปสั้นๆ

---

## 📊 ข่าวน่าสนใจ (MEDIUM Engagement)

### [หัวข้อ]
- **แหล่ง**: [เพจ/account]
- **Engagement**: X likes, Y comments, Z shares
- **เนื้อหา**: [สรุปสั้นๆ]

---

## 📈 Hashtag มาแรงวันนี้
- #... — กล่าวถึง X ครั้ง

---

## 📋 Accounts ที่ตรวจสอบ
### Facebook: X เพจ, Twitter/X: X account

_รายงานเมื่อ: YYYY-MM-DD HH:mm_
```

---

## Gotchas & Error Recovery

| ปัญหา | วิธีแก้ |
|-------|---------|
| ต้อง login | บอก user ให้ login ใน browser แล้วทำต่อ |
| โหลดไม่เสร็จ | Refresh → รอ → ลองใหม่ |
| Rate limited | รอ 30 วิ → ทำ account ต่อไป |
| โพสต์ถูกลบ | ข้าม → จดว่า "unavailable" |
| Comments ไม่โหลด | คลิก comment area → รอ → scroll |
| Facebook redirect URL ผิด | จดเป็นหมายเหตุใน report |

**ถ้าทำไม่สำเร็จ:**
1. screenshot ประเมินสถานการณ์
2. ข้าม account ที่มีปัญหา → ทำ account ต่อไป
3. จดข้อจำกัดใน report

---

## Browser Tips
- เปิด internal browser **ก่อน** ทำอย่างอื่น
- Screenshot ทุกขั้นตอนสำคัญ
- รอหน้าโหลดเสร็จ — ดู spinner
- ใช้ keyboard shortcut ได้ (เสถียรกว่า click)
- พักบ้างระหว่าง request — กัน rate limit
- Facebook/Twitter เป็น SPA — โหลดเนื้อหาค่อยๆ
