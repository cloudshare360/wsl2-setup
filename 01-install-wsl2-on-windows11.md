Here’s how to **install WSL2 on Windows 11** step-by-step:

---

### 🪟 **Step 1: Open PowerShell as Administrator**

1. Press **Start → type “PowerShell”**
2. Right-click **Windows PowerShell → Run as Administrator**

---

### 🧩 **Step 2: Run the WSL installation command**

In PowerShell, run:

```powershell
wsl --install
```

This will:

* Enable **Windows Subsystem for Linux (WSL)**
* Enable **Virtual Machine Platform**
* Install the **latest Ubuntu** distribution by default

---

### 🌀 **Step 3: Reboot your PC**

After installation completes, restart your computer when prompted.

---

### 🧠 **Step 4: Set up your Linux distribution**

Once restarted:

1. WSL will open automatically and finalize installation.
2. You’ll be prompted to **create a Linux username and password**.

---

### ⚙️ **Optional: Choose a different Linux distro**

If you prefer another distro (e.g., Debian, Kali, openSUSE):

```powershell
wsl --list --online
```

Then install one, for example:

```powershell
wsl --install -d Debian
```

---

### 🧾 **Step 5: Verify your setup**

Check your WSL version:

```powershell
wsl --status
```

Ensure it shows **Default Version: 2**

If not, set it manually:

```powershell
wsl --set-default-version 2
```

---

### 🧰 **Step 6: Update WSL**

Keep it up to date:

```powershell
wsl --update
```

---

Would you like me to include **GUI setup (RDP/XFCE)** so you can use a Linux desktop environment inside WSL2?
