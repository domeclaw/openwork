# THWork Installation Guide

## macOS Installation

### Option 1: Using DMG (Recommended)

1. **Remove quarantine attribute** (required for unsigned apps):
   ```bash
   xattr -d com.apple.quarantine THWork*.dmg
   ```

2. **Mount the DMG**:
   ```bash
   open THWork*.dmg
   ```

3. **Drag THWork.app to Applications folder**

4. **Open THWork.app**

   If you see a warning about unidentified developer:
   - Go to **System Settings → Privacy & Security**
   - Scroll down and click **"Open Anyway"**
   - Confirm to open the app

### Option 2: Using ZIP

1. **Remove quarantine attribute**:
   ```bash
   xattr -d com.apple.quarantine THWork*.zip
   ```

2. **Extract the ZIP**:
   ```bash
   unzip THWork*.zip
   ```

3. **Move THWork.app to Applications folder**:
   ```bash
   mv THWork.app /Applications/
   ```

4. **Open THWork.app** (follow steps above if warning appears)

### Alternative: Right-click Method

Instead of using `xattr`, you can:
1. **Right-click** (or Control-click) on THWork.app
2. Select **Open** from the context menu
3. Click **Open** in the dialog that appears
   - This only needs to be done once

---

## Windows Installation

1. **Download the .exe installer**
2. **Run the installer**
3. **Follow the installation wizard**

No special steps required for Windows.

---

## Why does macOS show "Damaged" warning?

THWork is not code-signed with an Apple Developer certificate. This is because:
- We're a small team without Apple Developer Program membership ($99/year)
- The app is fully functional and safe to use
- The "Damaged" message is actually just macOS Gatekeeper warning about unsigned apps

### Solutions:
1. ✅ Use `xattr` command (shown above)
2. ✅ Use right-click → Open method
3. ✅ Approve in System Settings → Privacy & Security

---

## Troubleshooting

### "THWork is damaged and can't be opened"

Run this command in Terminal:
```bash
xattr -cr /Applications/THWork.app
```

### "THWork can't be opened because it is from an unidentified developer"

1. Open **System Settings**
2. Go to **Privacy & Security**
3. Scroll down to find THWork
4. Click **"Open Anyway"**

### App still won't open

Try removing quarantine recursively:
```bash
xattr -d com.apple.quarantine /Applications/THWork.app
```

---

## System Requirements

### macOS
- **Architecture**: Apple Silicon (M1/M2/M3/M4)
- **macOS Version**: 12.0 (Monterey) or later
- **RAM**: 8 GB minimum

### Windows
- **Architecture**: x64
- **Windows Version**: Windows 10 or later
- **RAM**: 8 GB minimum

---

## Need Help?

If you encounter any issues, please:
- Open an issue on GitHub
- Check existing issues for solutions
- Provide your OS version and error message

---

**Thank you for using THWork!** 🎉
