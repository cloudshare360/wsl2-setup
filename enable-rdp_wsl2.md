✅ Nice — you’re almost there.
That error —

> `powershell.exe: command not found`
> means the script is being run **inside WSL**, but WSL doesn’t recognize Windows commands like `powershell.exe` by default unless the environment path is set correctly.

Let’s fix it the clean and modern way 👇

---

## 🧠 What’s Happening

* WSL Ubuntu doesn’t know where to find `powershell.exe`.
* We’ll fix it by:

  1. Using the full Windows path to PowerShell.
  2. Ensuring the port proxy setup runs **from Windows**, not Linux.

---

## ⚙️ Option 1: Use full path to PowerShell (simple fix)

Edit your script:

```bash
sudo nano /usr/local/bin/fixwslrdp.sh
```

Replace its content with this:

```bash
#!/bin/bash
WSL_IP=$(hostname -I | awk '{print $1}')
POWERSHELL_CMD="netsh interface portproxy add v4tov4 listenport=3389 listenaddress=0.0.0.0 connectport=3389 connectaddress=$WSL_IP"
"/mnt/c/Windows/System32/WindowsPowerShell/v1.0/powershell.exe" -Command "$POWERSHELL_CMD"
```

Save and run:

```bash
sudo chmod +x /usr/local/bin/fixwslrdp.sh
sudo /usr/local/bin/fixwslrdp.sh
```

✅ You should no longer see the “command not found” error.

---

## ⚙️ Option 2: Run from Windows side (safer for RDP stability)

Instead of calling PowerShell *inside* WSL, make a `.bat` file on Windows that sets the proxy automatically.

Create a file in Windows, e.g. `C:\wslrdp.bat`:

```bat
@echo off
for /f "tokens=*" %%i in ('wsl hostname -I') do set WSL_IP=%%i
netsh interface portproxy add v4tov4 listenport=3389 listenaddress=0.0.0.0 connectport=3389 connectaddress=%WSL_IP%
```

Then, after each reboot:

* Start WSL (so Ubuntu is running)
* Run `C:\wslrdp.bat` as **Administrator**

Then connect via Remote Desktop using `localhost` (since it now forwards port 3389 to WSL2 internally).

---

## ✅ Recap

| Fix      | Description                                              |
| -------- | -------------------------------------------------------- |
| Option 1 | Call PowerShell from inside WSL using full path          |
| Option 2 | Create Windows `.bat` to auto-forward port (recommended) |

---

Would you like me to show how to make the `.bat` file run **automatically when WSL starts**, so RDP is always ready?
