# nmap.md

## Basic Scan
```bash
nmap IP

## Default Script + Service Version
nmap -sC -sV IP

## Save Output
nmap -sC -sV -oN nmap.txt IP

## Full Port Scan
nmap -p- --min-rate 1000 IP

##Scan Specific Ports
nmap -p 21,22,80,443,445 IP

## UDp Scan (Slow)
nmap -sU IP

## OS Detection
nmap -O IP

## Aggressive
nmap -A IP

