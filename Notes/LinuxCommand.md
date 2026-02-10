

## Basic Commands
```bash
whoami
id
hostname
uname -a
pwd
ls -la

## Useful Enumeration
sudo -l
cat /etc/passwd
cat /etc/hosts
ip a
netstat -tulnp
ss -tulnp


## FileSearch
find / -name "*.conf" 2>/dev/null
find / -type f -name "*pass*" 2>/dev/null
find / -perm -4000 2>/dev/null

# Permissions

r = read
w = write
x = execute



