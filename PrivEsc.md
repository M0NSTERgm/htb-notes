# Privilege Escalation Notes (HTB)

## Linux PrivEsc Checklist
```bash
whoami
id
sudo -l
uname -a
cat /etc/passwd
find / -perm -4000 2>/dev/null


# Common targets:
-misconfigured sudo rules
-SUID binaries
-writable cronjobs
-passwords in config files



## Windows PrivEsc Checklist
whoami
whoami /priv
systeminfo
net user
net localgroup administrators


# Common targets
-unquoted service paths
-weak service permissions
-stored credentials
-AlwaysInstallElevated



