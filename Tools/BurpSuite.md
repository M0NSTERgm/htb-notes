# Burp Suite Notes

## Fungsi Utama
- Proxy: intercept request/response
- Repeater: kirim request manual
- Intruder: brute force / fuzzing
- Decoder: encode/decode
- Comparer: compare response

## Setup Proxy
Browser Proxy:
- HTTP: 127.0.0.1
- Port: 8080

## Hal yang sering dicek
- Cookie session
- Parameter id (IDOR)
- SQL injection
- File upload
- Authentication bypass

## Tips
- Perhatikan status code (200/302/403/500)
- Cek response length
- Coba ubah parameter kecil dulu (id=1 jadi id=2)
