Perfect — let’s **enable RDP (Remote Desktop)** on **Ubuntu running inside WSL2** cleanly and reliably.
Follow these steps exactly — this setup works on all modern WSL2 Ubuntu versions (20.04–24.04).

---

## ⚙️ Step 1. Install a lightweight desktop and xRDP

Run this in your Ubuntu (WSL2) terminal:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y xfce4 xfce4-goodies xrdp
```

When prompted to choose a display manager, pick **lightdm**.

---

## 🧩 Step 2. Configure xRDP to use XFCE

Create the session file:

```bash
echo xfce4-session > ~/.xsession
```

Restart xRDP:

```bash
sudo service xrdp restart
```

---

## 🌐 Step 3. Find your WSL2 IP address

```bash
hostname -I
```

Example output:

```
172.25.190.1
```

That’s the IP address you’ll RDP into.

---

## 🪟 Step 4. Connect via Windows Remote Desktop

1. Open **Remote Desktop Connection** (`mstsc.exe`) on Windows.
2. Enter the WSL2 IP address (e.g., `172.25.190.1`).
3. Log in using your **Ubuntu username** and **password**.

✅ You should now see the **XFCE desktop** running from WSL2.

---

## 🔁 Step 5. Make xRDP start automatically

Enable xRDP at boot:

```bash
sudo systemctl enable xrdp
```

---

## ⚡ Optional: Fix WSL2 IP changing

WSL2 IP resets each restart. To make RDP work every time, create a script:

### Create the file:

```bash
sudo nano /usr/local/bin/fixwslrdp.sh
```

### Paste this:

```bash
#!/bin/bash
WSL_IP=$(hostname -I | awk '{print $1}')
POWERSHELL_CMD="netsh interface portproxy add v4tov4 listenport=3389 listenaddress=0.0.0.0 connectport=3389 connectaddress=$WSL_IP"
powershell.exe -Command "$POWERSHELL_CMD"
```

### Make it executable:

```bash
sudo chmod +x /usr/local/bin/fixwslrdp.sh
```

Run this script whenever you restart WSL before connecting via RDP:

```bash
sudo /usr/local/bin/fixwslrdp.sh
```

---

## ✅ Quick Recap

| Step | Command                            | Purpose                 |
| ---- | ---------------------------------- | ----------------------- |
| 1    | `sudo apt install xfce4 xrdp`      | Install UI + RDP server |
| 2    | `echo xfce4-session > ~/.xsession` | Configure session       |
| 3    | `sudo service xrdp restart`        | Start xRDP              |
| 4    | `hostname -I`                      | Get IP to RDP into      |
| 5    | `mstsc.exe` → use IP               | Connect                 |
| 6    | Optional script                    | Fix IP reset issue      |

---

Would you like me to make this **auto-start + auto-IP-fix** every time WSL2 launches (no manual steps)?
