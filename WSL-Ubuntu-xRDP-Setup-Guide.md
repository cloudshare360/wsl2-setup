Below is a **clean, consolidated, production-ready Markdown guide** you can save in your Git repo.

It includes:

✔ WSL user setup
✔ Password reset
✔ xRDP configuration
✔ Multi-user & multi-session support
✔ DPI & screen scaling fixes (terminal commands only)
✔ Troubleshooting

---

# 🧾 **WSL Ubuntu + xRDP Full Setup Guide (Multi-User, Working DPI, No Scrollbars)**

This guide documents how to properly configure and fix xRDP on **WSL Ubuntu**, including:

* Multi-user login (`sri`, `sricloud`)
* RDP login failures
* WSL session limitations
* DPI/font scaling
* Screen resolution fixes
* Runtime directory requirements

---

# 1️⃣ **Verify Linux Users Exist**

```bash
getent passwd | grep -E "sri|sricloud"
```

Expected:

```
sri:x:1000:1000:/home/sri:/bin/bash
sricloud:x:1001:1001:/home/sricloud:/bin/bash
```

---

# 2️⃣ **Reset Passwords (From root)**

```bash
passwd sri
passwd sricloud
```

Test password:

```bash
su - sri
sudo whoami
```

Should output:

```
root
```

Repeat for `sricloud`.

---

# 3️⃣ **Create Required xRDP Runtime Directories (WSL Required)**

WSL does NOT create `/run/user/<UID>` → xRDP login fails.

```bash
sudo mkdir -p /run/user/1000
sudo mkdir -p /run/user/1001

sudo chmod 700 /run/user/1000 /run/user/1001
sudo chown sri:sri /run/user/1000
sudo chown sricloud:sricloud /run/user/1001
```

---

# 4️⃣ **Fix xRDP Start-up (WSL DBUS & XDG Issues)**

Edit:

```bash
sudo nano /etc/xrdp/startwm.sh
```

Add **at the top**:

```bash
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
```

Add **before the final line (`exec startxfce4`)** if you want default scaling:

```bash
# Optional defaults
# xrandr --dpi 120
# xrandr --size 1920x1080
```

Save + exit.

---

# 5️⃣ **Enable Unlimited Sessions (Multi-Session Support)**

```bash
sudo nano /etc/xrdp/sesman.ini
```

Modify:

```
MaxSessions=0
KillDisconnectedSessions=false
```

Restart services:

```bash
sudo service xrdp-sesman restart
sudo service xrdp restart
```

---

# 6️⃣ **Correct Way to Start WSL Before RDP Login**

WSL must be started **as the same user you plan to RDP into**.

Example for user `sri`:

```bash
wsl --shutdown
wsl -u sri
whoami   # should output: sri
```

Now RDP with:

```
username: sri
password: <sri-password>
```

For user `sricloud`:

```bash
wsl --shutdown
wsl -u sricloud
```

---

# 7️⃣ **Fix Font Size, DPI, and UI Scaling (Terminal Only)**

### ✔ Set DPI

```bash
xfconf-query -c xsettings -p /Xft/DPI -n -t int -s 120
```

### ✔ Optional: Larger text

```bash
xfconf-query -c xsettings -p /Xft/DPI -t int -s 144
```

---

### ✔ Apply ScaleFactor (makes fonts visibly bigger)

```bash
xfconf-query -c xsettings -p /Xft/ScaleFactor -n -t double -s 1.4
```

Try bigger if needed:

```bash
xfconf-query -c xsettings -p /Xft/ScaleFactor -t double -s 1.6
```

---

### ✔ UI Window Scaling (buttons, icons, menus)

```bash
xfconf-query -c xsettings -p /Gtk/WindowScalingFactor -n -t int -s 2
```

---

### ✔ Refresh settings without logout

```bash
xfsettingsd --replace &
```

---

# 8️⃣ **Fix Screen Resolution (No Scrollbars in RDP)**

Check available resolutions:

```bash
xrandr
```

Apply one:

```bash
xrandr --size 1920x1080
```

Or:

```bash
xrandr --size 1366x768
```

---

# 9️⃣ **Optional: Set Terminal Font**

```bash
xfconf-query -c xfce4-terminal -p /general/font-name -s "DejaVu Sans Mono 16"
```

---

# 🔟 **Troubleshooting**

### ❌ RDP shows “login failed”

Check what username RDP is sending:

```bash
tail -f /var/log/xrdp-sesman.log
```

You must see:

```
login attempt for user: sri
```

If you see:

* sri1
* sri@hostname
* HOST\sri
* sri_

→ RDP is sending wrong username.

Clear credentials:

* Open **Credential Manager → Windows Credentials → remove 127.0.0.1 entries**
* Re-open RDP
* Enter username **exactly** as `sri`

---

### ❌ Keeps asking for password / fails

Check password locally:

```bash
su - sri
sudo whoami
```

---

### ❌ Black screen after login

Missing runtime directory — fix Step 3.

---

### ❌ Scrollbars in RDP

Set resolution:

```bash
xrandr --size 1920x1080
```

---

### ❌ Font still too small

Increase:

```bash
xfconf-query -c xsettings -p /Xft/ScaleFactor -t double -s 1.6
```

---

# 🎉 **This Markdown is ready to save to your repository.**

Do you want a shorter “cheat sheet” version also?
