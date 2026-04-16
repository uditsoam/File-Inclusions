# PHP Filters in LFI – Notes

## 1. Problem

When including PHP files via LFI:
- Files get executed
- Source code is not visible

---

## 2. Solution: PHP Filters

PHP provides wrappers using:
```
php://filter
```

---

## 3. Base64 Filter

### Payload
```
php://filter/read=convert.base64-encode/resource=config
```

---

## 4. How it Works

- Prevents execution of PHP file
- Encodes file content in base64
- Allows reading source code

---

## 5. Exploitation Steps

### Step 1: Get encoded output
```
?language=php://filter/read=convert.base64-encode/resource=config
```

### Step 2: Decode
```
echo "encoded_string" | base64 -d
```

---

## 6. Use Cases

- Extract credentials
- Read hidden files
- Analyze application logic
- Find vulnerabilities

---

## 7. Key Takeaways

- php://filter is a powerful LFI technique
- Works best when PHP execution hides content
- Essential for advanced exploitation
