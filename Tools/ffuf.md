# FFUF Cheatsheet

## Directory Fuzzing
```bash
ffuf -u http://IP/FUZZ -w /usr/share/wordlists/dirb/common.txt

## With Extensions
ffuf -u http://IP/FUZZ -w WORDLIST -e .php,.txt,.html

## Filter Status
ffuf -u http://IP/FUZZ -w WORDLIST -mc 200,204,301,302,403
