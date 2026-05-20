# THWork Rebrand Guide

คู่มือนี้สรุปขั้นตอนทั้งหมดที่ต้องทำเพื่อ rebrand จาก OpenWork → THWork (หรือจาก THWork → brand อื่น)

---

## 📋 สรุปไฟล์ที่ต้องแก้ไข

### 1. **Desktop App (Electron)**

#### `apps/desktop/package.json`
- **line 2:** `"name"` - ชื่อ package (เช่น `"@openwork/desktop"`)
- **line 4:** `"version"` - version number
- **line 5:** `"description"` - app description

```json
{
  "name": "@openwork/desktop",
  "version": "0.13.12",
  "description": "THWork desktop shell",
}
```

#### `apps/desktop/electron/main.mjs`
- **line 55:** `APP_NAME` - ชื่อ app ที่แสดงใน UI
- **line 2934-2940:** macOS About panel configuration
  - `applicationName`
  - `copyright`

```javascript
const APP_NAME = "THWork";

app.setAboutPanelOptions({
  applicationName: APP_NAME,
  applicationVersion: APP_VERSION,
  copyright: "© 2026 THWork",
});
```

#### `apps/desktop/electron-builder.yml`
- **line 4:** `appId` - macOS bundle identifier
- **line 5:** `productName` - ชื่อ app
- **line 7-8:** `protocols` - URL scheme

```yaml
appId: com.differentai.openwork
productName: THWork
protocols:
  - name: THWork
    schemes:
      - openwork
```

---

### 2. **App Icons & Logos**

#### ไฟล์ที่ต้องเปลี่ยน:

| ไฟล์ | ตำแหน่ง | รูปแบบ |
|------|---------|--------|
| **macOS Icon** | `apps/desktop/resources/icons/thwork.icns` | `.icns` (512x512+) |
| **Windows Icon** | `apps/desktop/resources/icons/thwork.ico` | `.ico` (256x256) |
| **Linux Icon** | `apps/desktop/resources/icons/thwork.png` | `.png` (512x512+) |
| **SVG Logo** | `apps/app/public/openwork-logo.svg` | `.svg` |
| **SVG Logo Square** | `apps/app/public/openwork-logo-square.svg` | `.svg` |
| **Favicon 16x16** | `apps/app/public/favicon-16x16.png` | `.png` |
| **Favicon 32x32** | `apps/app/public/favicon-32x32.png` | `.png` |
| **Apple Touch Icon** | `apps/app/public/apple-touch-icon.png` | `.png` (180x180) |

#### Icon Generation Tools:
- **macOS (.icns):** ใช้ `iconutil` หรือ https://cloudconvert.com/png-to-icns
- **Windows (.ico):** ใช้ https://convertio.co/png-ico/
- **Linux (.png):** PNG ธรรมดา ขนาด 512x512

#### สร้าง icon set สำหรับ macOS:
```bash
# สร้าง icon.iconset folder
mkdir icon.iconset

# สร้าง PNG sizes
sips -z 16 16 logo.png --out icon.iconset/icon_16x16.png
sips -z 32 32 logo.png --out icon.iconset/icon_16x16@2x.png
sips -z 32 32 logo.png --out icon.iconset/icon_32x32.png
sips -z 64 64 logo.png --out icon.iconset/icon_32x32@2x.png
sips -z 128 128 logo.png --out icon.iconset/icon_128x128.png
sips -z 256 256 logo.png --out icon.iconset/icon_128x128@2x.png
sips -z 256 256 logo.png --out icon.iconset/icon_256x256.png
sips -z 512 512 logo.png --out icon.iconset/icon_256x256@2x.png
sips -z 512 512 logo.png --out icon.iconset/icon_512x512.png
sips -z 1024 1024 logo.png --out icon.iconset/icon_512x512@2x.png

# Convert เป็น .icns
iconutil -c icns icon.iconset
```

---

### 3. **Shell Configuration**

#### `apps/app/src/react-app/shell/shell-config.tsx`
- **line 38:** `appName` - ชื่อ app ใน UI

```typescript
export const DEFAULT_SHELL_CONFIG: ShellConfig = {
  appName: "THWork",
  statusBar: true,
  sidebar: true,
  // ... อื่นๆ
};
```

---

### 4. **i18n Localization**

#### `apps/app/src/i18n/locales/en.ts`
- **line 56-57:** Blueprint welcome messages
- **line 729:** Welcome page title

```typescript
"blueprint.welcome_message": "Hi welcome to THWork!\n\n...",
"blueprint.welcome_title": "Welcome to THWork",
// ...
"welcome.title": "Welcome to THWork",
```

#### `apps/app/src/i18n/locales/th.ts`
- **line 55-56:** Blueprint welcome messages (Thai)

```typescript
"blueprint.welcome_message": "สวัสดี ยินดีต้อนรับสู่ THWork!...",
"blueprint.welcome_title": "ยินดีต้อนรับสู่ THWork",
```

---

### 5. **Welcome Page Capabilities**

#### `apps/app/src/react-app/domains/onboarding/welcome-page.tsx`
- **line 35-66:** Capabilities array - title & desc ของแต่ละ capability

```typescript
const capabilities = [
  {
    slug: "googlechrome",
    title: "ข่าว Hot!! ล่าสุดจากเพจทีวีพูล",
    desc: "ค้นหาโพสต์ล่าสุดและวิเคราะห์ engagement.",
  },
  // ...
];
```

---

### 6. **Starter Prompts**

#### `apps/app/src/react-app/domains/session/chat/session-page.tsx`
- **line 65-70:** Prompt examples ที่แสดงใน homepage

```typescript
const starterPrompts = [
  "ค้นหาข่าวทีวีพูลล่าสุด 5 โพสต์ พร้อมสรุป",
  // ...
];
```

#### `apps/app/src/react-app/domains/session/chat/session-surface.tsx`
- **line 44-53:** Prompt examples ที่แสดงใน empty session

```typescript
const suggestions = [
  "ค้นหาข่าวทีวีพูลล่าสุด 5 โพสต์ พร้อมสรุป",
  // ...
];
```

---

### 7. **URL Scheme & Protocol**

#### `apps/desktop/electron-builder.yml`
```yaml
protocols:
  - name: THWork
    schemes:
      - openwork  # URL scheme (เช่น openwork://import-bundle)
```

---

### 8. **Bundle ID & Identifiers**

#### `apps/desktop/electron-builder.yml`
```yaml
appId: com.differentai.openwork  # macOS bundle ID
```

**หมายเหตุ:** เปลี่ยน appId จะทำให้ macOS มองว่าเป็น app ใหม่ ทำให้:
- Keychain entries ไม่ shared
- ข้อมูล app ไม่ migrate อัตโนมัติ
- ต้อง uninstall app เก่าก่อน

---

### 9. **Embedded Skills**

#### `apps/desktop/resources/skills/`
- วาง skill folders ที่ต้องการ embed ใน app
- Skills จะถูก copy อัตโนมัติตอนสร้าง workspace ใหม่

```
resources/skills/
└── tivi-pools-news/
    └── SKILL.md
```

---

## ✅ Checklist การ Rebrand

### Phase 1: Core Identity
- [ ] เปลี่ยน `APP_NAME` ใน `main.mjs`
- [ ] เปลี่ยน `appName` ใน `shell-config.tsx`
- [ ] เปลี่ยน `description` ใน `package.json`
- [ ] เปลี่ยน `productName` ใน `electron-builder.yml`

### Phase 2: Icons & Logos
- [ ] สร้างและแทนที่ `thwork.icns` (macOS)
- [ ] สร้างและแทนที่ `thwork.ico` (Windows)
- [ ] สร้างและแทนที่ `thwork.png` (Linux)
- [ ] สร้างและแทนที่ SVG logos
- [ ] สร้างและแทนที่ favicons
- [ ] สร้างและแทนที่ Apple touch icon

### Phase 3: UI Text
- [ ] เปลี่ยน welcome messages ใน i18n (en.ts, th.ts)
- [ ] เปลี่ยน capabilities ใน `welcome-page.tsx`
- [ ] เปลี่ยน starter prompts ใน `session-page.tsx`
- [ ] เปลี่ยน starter prompts ใน `session-surface.tsx`

### Phase 4: About Dialog
- [ ] เปลี่ยน copyright ใน `main.mjs`
- [ ] ตรวจสอบ version display

### Phase 5: Branding URLs (ถ้ามี)
- [ ] เปลี่ยน docs URL
- [ ] เปลี่ยน feedback URL
- [ ] เปลี่ยน signin URL

### Phase 6: Build & Test
- [ ] Build app ใหม่
- [ ] ตรวจสอบ icon ใน About dialog
- [ ] ตรวจสอบ icon ใน Dock/Taskbar
- [ ] ตรวจสอบ icon ใน DMG
- [ ] ทดสอบ create workspace ใหม่
- [ ] ตรวจสอบ embedded skills

---

## 🔍 วิธีตรวจสอบว่า Rebrand ครบแล้ว

### 1. ค้นหาข้อความ brand เก่า:
```bash
grep -r "OpenWork" apps/ --include="*.tsx" --include="*.ts" --include="*.mjs" --include="*.json" | grep -v node_modules | grep -v ".opencode"
```

### 2. ตรวจสอบ icon files:
```bash
find apps/desktop/resources/icons -type f
find apps/app/public -name "*.png" -o -name "*.svg" | grep -E "(logo|favicon|touch)"
```

### 3. ตรวจสอบ About dialog:
- เปิด app → เมนู THWork → About THWork
- ตรวจสอบ: ชื่อ, version, copyright

### 4. ตรวจสอบ welcome page:
- สร้าง workspace ใหม่
- ตรวจสอบ welcome message และ capabilities

---

## ⚠️ ข้อควรระวัง

### อย่าเปลี่ยน:
- ❌ ตัวแปรชื่อ `openwork` ใน code
- ❌ Function names ที่มี `openwork`
- ❌ HTTP headers (เช่น `X-OpenWork-Host-Token`)
- ❌ Module names
- ❌ Database table names
- ❌ File structure ที่ใช้ `openwork`

**เหตุผล:** เพื่อรักษา backward compatibility และไม่ให้ breaking changes

### ต้องเปลี่ยน:
- ✅ UI text ทั้งหมดที่ผู้ใช้เห็น
- ✅ Icons และ logos
- ✅ App descriptions
- ✅ Welcome messages
- ✅ Error messages
- ✅ Documentation strings

---

## 📝 ตัวอย่างการเปลี่ยนชื่อ Brand

### จาก THWork → MyBrand

1. **เปลี่ยนชื่อ:**
   ```javascript
   // main.mjs
   const APP_NAME = "MyBrand";
   
   // shell-config.tsx
   appName: "MyBrand",
   
   // electron-builder.yml
   productName: MyBrand
   ```

2. **เปลี่ยน icons:**
   - แทนที่ไฟล์ทั้งหมดใน `resources/icons/`
   - ตั้งชื่อเป็น `mybrand.icns`, `mybrand.ico`, `mybrand.png`

3. **เปลี่ยน i18n:**
   ```typescript
   "blueprint.welcome_title": "ยินดีต้อนรับสู่ MyBrand"
   ```

4. **Build:**
   ```bash
   pnpm --filter @openwork/desktop run package:electron
   ```

---

## 🎨 Icon Requirements

### macOS (.icns)
- ความละเอียดขั้นต่ำ: 512x512 pixels
- แนะนำ: 1024x1024 pixels
- รองรับ transparency

### Windows (.ico)
- ความละเอียดขั้นต่ำ: 256x256 pixels
- ควรมีหลาย sizes: 16, 32, 48, 64, 128, 256
- รองรับ transparency

### Linux (.png)
- ความละเอียด: 512x512 pixels
- รองรับ transparency

### Web (SVG)
- Vector format
- รองรับ dark/light mode

---

## 📦 Build Command

```bash
# Build macOS
pnpm --filter @openwork/desktop run package:electron

# Output:
# - dist-electron/openwork-mac-arm64-<version>.dmg
# - dist-electron/openwork-mac-arm64-<version>.zip
# - dist-electron/mac-arm64/THWork.app
```

---

## 🧪 Testing Checklist

หลัง rebrand และ build เสร็จ:

- [ ] App เปิดได้ไม่มี error
- [ ] Icon ใน Dock/Taskbar ถูกต้อง
- [ ] About dialog แสดงชื่อและ icon ถูกต้อง
- [ ] Welcome page แสดงข้อความ brand ใหม่
- [ ] Create workspace ใหม่ได้
- [ ] Embedded skills ถูก copy ไป
- [ ] ไม่มีข้อความ brand เก่าเหลืออยู่ใน UI
- [ ] Error messages ใช้ brand ใหม่

---

## 📚 ไฟล์ที่เกี่ยวข้องทั้งหมด

```
apps/desktop/
├── package.json                          # app description
├── electron-builder.yml                  # productName, appId, icons
├── electron/
│   └── main.mjs                          # APP_NAME, About panel
└── resources/
    ├── icons/
    │   ├── thwork.icns                   # macOS icon
    │   ├── thwork.ico                    # Windows icon
    │   └── thwork.png                    # Linux icon
    └── skills/                           # embedded skills

apps/app/
├── src/react-app/
│   ├── shell/
│   │   └── shell-config.tsx              # appName
│   └── domains/
│       ├── onboarding/
│       │   └── welcome-page.tsx          # capabilities
│       └── session/chat/
│           ├── session-page.tsx          # starter prompts
│           └── session-surface.tsx       # starter prompts
├── src/i18n/locales/
│   ├── en.ts                             # English welcome messages
│   └── th.ts                             # Thai welcome messages
└── public/
    ├── openwork-logo.svg                 # SVG logo
    ├── openwork-logo-square.svg          # Square logo
    ├── favicon-16x16.png                 # Favicon
    ├── favicon-32x32.png                 # Favicon
    └── apple-touch-icon.png              # iOS icon
```

---

## 🎯 Quick Reference

| สิ่งที่ต้องเปลี่ยน | ไฟล์ | บรรทัด |
|-------------------|------|--------|
| App name (runtime) | `main.mjs` | 55 |
| App name (config) | `shell-config.tsx` | 38 |
| App name (builder) | `electron-builder.yml` | 5 |
| About dialog | `main.mjs` | 2934-2940 |
| Welcome title (EN) | `en.ts` | 56-57, 729 |
| Welcome title (TH) | `th.ts` | 55-56 |
| Capabilities | `welcome-page.tsx` | 35-66 |
| Starter prompts | `session-page.tsx` | 65-70 |
| Starter prompts | `session-surface.tsx` | 44-53 |
| macOS icon | `resources/icons/*.icns` | - |
| Windows icon | `resources/icons/*.ico` | - |
| Linux icon | `resources/icons/*.png` | - |

---

**Last updated:** May 2026  
**Version:** 0.13.12  
**Brand:** THWork
