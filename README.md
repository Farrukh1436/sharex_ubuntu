# 🖼️ Auto Screenshot System (Ubuntu)  
> ShareX-style screenshot automation for Ubuntu with watermark, counter naming, and custom hotkey  

---

## ✨ Features
- 📸 **Automatic screenshots** using Flameshot  
- 🔢 **Auto-incremented filenames** (`shot_001.png`, `shot_002.png`, …)  
- 🧾 **Bold bordered watermark** at top-center  
- 🎹 **Custom hotkey integration**  
- 💾 Saves all screenshots in `~/Pictures/Screenshots`  
- ⚙️ Works perfectly with both X11 and Wayland  

---

## 🧰 Requirements
Install the required packages:
```bash
sudo apt update
sudo apt install flameshot imagemagick
```

---

## ⚙️ Installation Steps

### 1️⃣ Create Script Directory
```bash
mkdir -p ~/.local/bin
```

### 2️⃣ Create the Screenshot Script
Create the file:
```bash
nano ~/.local/bin/snap_with_name.sh
```

Paste this content:

```bash
#!/bin/bash
# Flameshot auto screenshot with name and counter

SAVE_DIR="$HOME/Pictures/Screenshots"
mkdir -p "$SAVE_DIR"

# Count existing PNGs to create sequential filenames
COUNTER=$(ls "$SAVE_DIR"/*.png 2>/dev/null | wc -l)
FILENAME=$(printf "%s/%03d_shot.png" "$SAVE_DIR" $((COUNTER + 1)))

# Take full screenshot and save
flameshot full -p "$FILENAME"

convert "$FILENAME" \
  -gravity north \
  -pointsize 76 \
  -font DejaVu-Sans-Bold \
  -fill white \
  -stroke black -strokewidth 3 \
  -annotate +0+40 "Farrukh Sattorov | U2310237 | Section:1 | Member:3" \
  "$FILENAME"


# Optional notification
notify-send "📸 Screenshot saved: $(basename "$FILENAME")"
```

Save (`Ctrl + O`, `Enter`) and exit (`Ctrl + X`).

Make it executable:
```bash
chmod +x ~/.local/bin/snap_with_name.sh
```

---

### 3️⃣ Test Manually
```bash
~/.local/bin/snap_with_name.sh
```
Check your folder:
```bash
ls ~/Pictures/Screenshots
```

---

### 4️⃣ Create a Custom Shortcut
1. Open **Settings → Keyboard → Keyboard Shortcuts**  
2. Scroll down and click **“Custom Shortcuts → Add Shortcut”**  
3. Fill in:
   - **Name:** Screenshot with Name  
   - **Command:** `/home/$USER/.local/bin/snap_with_name.sh`  
   - **Shortcut:** `Shift + F1` (or any you like)  
4. Click **Add** ✅

---

## 🧩 Optional: Region Selection
If you want to **select a specific region**, replace this line in the script:
```bash
flameshot full -p "$FILENAME"
```
with:
```bash
flameshot gui -p "$SAVE_DIR"
```
*(You can still add the watermark afterward using the same convert command.)*

---

## 🖋️ Example Output

**Watermark example:**  
```
Farrukh Sattorov | U2310237 | Section:1 | Member:3
```
Appears **bold, white, black-bordered**, and perfectly centered at the top of every screenshot.

---

## 🧾 Example Usages

### 🖼️ Screenshot Preview
Below is an example of how the watermark appears on the screenshot:

![Example Screenshot](example_output.png)

The text appears **bold**, **white**, and surrounded by a **black border**, aligned perfectly at the **top center**.

---

### 🖥️ Terminal Demo (Optional)
Record your screen to show the workflow using `peek`:

```bash
sudo apt install peek
peek
```

Then embed it into your README:
```markdown
![Demo](demo.gif)
```

---

### ⚡ Command Usage
You can also manually run:
```bash
~/.local/bin/snap_with_name.sh
```
Or use your assigned hotkey (e.g., `Shift + F1`) for instant capture.  
Each time, a new file (`shot_001.png`, `shot_002.png`, …) is saved to your `~/Pictures/Screenshots` folder.

---

## 💡 Notes
- Works on Ubuntu 22.04–25.04 and derivatives (Pop!_OS, Mint, KDE Neon, etc.)  
- You can edit your name, font size, or position by tweaking the `convert` command line.  
- Each new screenshot auto-increments file number — no overwriting.

---

## 🧑‍💻 Credits
Developed by **Farrukh Sattorov**  
> Inspired by ShareX functionality for Linux systems.

---

## 📄 License
MIT License — free to use, modify, and share.