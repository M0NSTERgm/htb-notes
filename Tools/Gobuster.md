# Gobuster Cheatsheet

## Directory Bruteforce
```bash
gobuster dir -u http://IP -w /usr/share/wordlists/dirb/common.txt

## With Extensions
gobuster dir -u http://IP -w WORDLIST -x php,html,txt

## Add Threads
gobuster dir -u http://IP -w WORDLIST -t 50

## Use Status Code
gobuster dir -u http://IP -w WORDLIST -s 200,204,301,302,307,403

Notes:
- 301/302 = redirect (biasanya menarik)
- 403 = forbidden (file ada tapi diblok)
