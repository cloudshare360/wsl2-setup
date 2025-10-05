Here’s the **recommended way to install Google Chrome on Ubuntu** — using the official Google repository for automatic updates:

---

### ✅ **Method: Install Google Chrome via Official APT Repository**

#### 1. **Download and add Google’s signing key**
```bash
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /etc/apt/keyrings/google-linux-signing-key.gpg
```

> 🔒 This places the key in the secure `/etc/apt/keyrings/` directory (modern best practice).

#### 2. **Add the Google Chrome repository**
```bash
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/google-linux-signing-key.gpg] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
```

#### 3. **Update package list**
```bash
sudo apt update
```

#### 4. **Install Google Chrome**
Choose **one** of the following:

- **Stable version (recommended):**
  ```bash
  sudo apt install google-chrome-stable
  ```

- *Optional:* Beta or unstable versions:
  ```bash
  sudo apt install google-chrome-beta
  # or
  sudo apt install google-chrome-unstable
  ```

---

### 🚀 Launch Chrome
- **GUI**: Search for "Google Chrome" in your applications menu.
- **Terminal**: Run `google-chrome`

---

### 🔁 **Automatic Updates**
Since you added the official repo, Chrome will update automatically with:
```bash
sudo apt update && sudo apt upgrade
```

---

### ❌ Avoid: Manual `.deb` Download (Not Recommended Long-Term)

While you *can* download the `.deb` file from [https://www.google.com/chrome/](https://www.google.com/chrome/) and install it with:
```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb
```

⚠️ **Problem**: This does **not** set up the repository, so you **won’t get automatic updates**. The method above is better.

---

### 🧪 Verify Installation
```bash
google-chrome --version
```
Output example:
```
Google Chrome 129.0.6668.90
```

---

### 💡 Tip: Set Chrome as Default Browser (Optional)
After launching Chrome:
1. Click **⋮ (Menu)** → **Settings**
2. Go to **Default browser** → **Make default**

Or via system settings:
- **Settings** → **Default Applications** → **Web** → Choose **Google Chrome**

---

✅ You now have **Google Chrome installed securely with auto-updates** on Ubuntu!

Let me know if you'd like help installing Chromium (open-source alternative) or configuring Chrome for development! 🌐
