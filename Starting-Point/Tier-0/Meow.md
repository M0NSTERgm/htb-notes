# HTB Starting Point - Meow

**Level:** Tier 0 (Pemula)  
**Kategori:** Telnet / Misconfiguration  
**Platform:** Hack The Box - Starting Point

## Ringkasan
Mesin *Meow* mengajarkan dasar pentesting yaitu melakukan scanning port menggunakan Nmap, mengenali service Telnet, lalu memanfaatkan konfigurasi buruk (misconfiguration) karena Telnet mengizinkan login root tanpa password.

## Tools yang digunakan
- Nmap
- Telnet

## Catatan
Writeup ini dibuat hanya untuk pembelajaran dan tidak untuk aktivitas ilegal.

# HTB Starting Point - Meow (Writeup Bahasa Indonesia)

## Informasi Target
- **Nama Mesin:** Meow
- **IP Target:** 10.129.153.172

---

# 1. Reconnaissance (Scanning)

Langkah pertama adalah melakukan scanning untuk melihat port apa saja yang terbuka pada target.

Command:
```bash
nmap -sC -sV -oN 10.129.153.172
```

Hasil utama yang ditemukan:
- 23/tcp open telnet

# 2. Enumeration

Telnet adalah protokol remote login lama yang tidak aman karena mengirim username dan password tanpa enkripsi (plaintext).
Jika Telnet terbuka di internet, maka sangat berbahaya karena bisa diakses oleh siapa saja.

langkah berikutnya adalah mencoba koneksi langsung.

Command:
```bash
telnet 10.129.153.172
```
Setelah berhasil terhubung, target menampilkan prompt login.

# 3. Exploitation (Mendapatkan Akses)

Karena ini mesin Tier 0, biasanya terdapat konfigurasi buruk seperti default credential.

Login yang dicoba:
```bash
Username:
root

Password:
(kosong / langsung tekan ENTER)
```
Login berhasil dan sistem memberikan akses shell sebagai root.

Verifikasi akses:
```bash
whoami
id
```
Hasil menunjukkan:
```bash
user = root
uid = 0 (root)
```
Ini artinya kita sudah mendapatkan akses tertinggi.

# 4. Mengambil Flag

Biasanya flag berada di direktori root.

Command:
```bash
cd /root
ls -la
cat flag.txt
```

Flag ditemukan pada file flag.txt.
```bash
HTB{*******************************}
```
# 5. Analisis Kerentanan
Penyebab Masalah

Target memiliki service Telnet yang terbuka dan mengizinkan login root tanpa password.

Ini termasuk:
-Misconfiguration
-Weak Authentication
-Insecure Protocol (Telnet)

# 6. Mitigasi (Perbaikan Keamanan)

Langkah yang seharusnya dilakukan administrator:

-Menonaktifkan Telnet service
-Menggunakan SSH sebagai pengganti Telnet
-Menonaktifkan login root secara remote
-Menggunakan password yang kuat
-Membatasi akses port menggunakan firewall
