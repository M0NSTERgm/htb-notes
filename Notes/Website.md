# Web Notes (HTB)

## Common Vulnerabilities
- SQL Injection (SQLi)
- Cross Site Scripting (XSS)
- Local File Inclusion (LFI)
- Remote File Inclusion (RFI)
- Command Injection
- File Upload Vulnerability
- IDOR (Insecure Direct Object Reference)

## Useful Checks
- robots.txt
- sitemap.xml
- hidden directories
- backup files (.bak, .zip)

## Enumeration
```bash
whatweb http://IP
gobuster dir -u http://IP -w WORDLIST

# Tools
-Burp Suite
-ffuf
-gobuster
-nikto
