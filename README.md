# File-Inclusions
# Local File Inclusion (LFI) 

## 1. Introduction
Local File Inclusion (LFI) is a web vulnerability that occurs when a web application includes files based on user-controlled input without proper validation.

It allows an attacker to read local files from the server and, in some cases, execute code.

---

## 2. How LFI Works
Modern web applications use dynamic content loading. Instead of creating multiple pages, they use a single template and load content using parameters.

### Example:
```
http://example.com/index.php?page=about
```

Backend behavior:
```
include("about.php");
```

If user input is not validated, an attacker can manipulate it:
```
http://example.com/index.php?page=/etc/passwd
```

---

## 3. Root Cause
The main cause of LFI is:
- User input directly passed into file-handling functions
- No validation or sanitization
- Dynamic file inclusion logic

---

## 4. Vulnerable Code Examples

### PHP
```php
if (isset($_GET['language'])) {
    include($_GET['language']);
}
```

### NodeJS
```javascript
fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
    res.write(data);
});
```

### Java (JSP)
```jsp
<jsp:include file="<%= request.getParameter('language') %>" />
```

### .NET
```csharp
Response.WriteFile(Request.Query["language"]);
```

---

## 5. Impact of LFI

### 1. Sensitive File Disclosure
- /etc/passwd
- Configuration files
- System files

### 2. Source Code Exposure
- Helps attackers find more vulnerabilities

### 3. Credential Leakage
- Database usernames/passwords
- API keys

### 4. Remote Code Execution (RCE)
- Possible in advanced scenarios

---

## 6. Read vs Execute

| Function Type | Behavior |
|--------------|---------|
| Read         | Displays file content |
| Execute      | Executes file |

### Examples

#### Read Only
```php
file_get_contents()
```

#### Execute
```php
include()
require()
```

---

## 7. Common LFI Entry Points
- ?page=
- ?file=
- ?lang=
- ?template=

---

## 8. Basic Exploitation

### Step 1: Identify Parameter
```
?page=
```

### Step 2: Test Path Traversal
```
../../../../etc/passwd
```

### Step 3: Confirm Vulnerability
- If file content appears → LFI confirmed

---

## 9. Advanced Exploitation
- Log poisoning
- File upload + inclusion
- PHP wrappers
- Converting LFI to RCE

---

## 10. Key Takeaways
- LFI is caused by insecure file inclusion
- Exists in all major languages (PHP, NodeJS, Java, .NET)
- Can lead to full system compromise
- Understanding file handling functions is critical

---

## 11. Quick Summary
- LFI = User-controlled file inclusion
- Main issue = Lack of input validation
- Impact = File read → Credential leak → RCE
