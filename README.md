# 🛡️ Cross-Site Scripting (XSS) – Practical Lab & Defense Guide

## 📌 Overview

This project demonstrates a practical understanding of **Cross-Site Scripting (XSS)**, one of the most common web security vulnerabilities, as defined by OWASP.

The goal of this lab is to:

* Understand how XSS works
* Perform a safe simulation on a local application
* Analyze real-world impact
* Implement proper fixes

---

## ⚠️ What is XSS?

**Cross-Site Scripting (XSS)** is a vulnerability where attackers inject malicious JavaScript into web applications, which then executes in other users’ browsers.

---

## 🧪 Lab Setup

### Vulnerable Code Example

```html
<!DOCTYPE html>
<html>
<body>

<h2>Comment Section</h2>

<input type="text" id="userInput" />
<button onclick="addComment()">Post</button>

<div id="comment"></div>

<script>
function addComment() {
  const input = document.getElementById("userInput").value;

  // ❌ Vulnerable code
  document.getElementById("comment").innerHTML += "<p>" + input + "</p>";
}
</script>

</body>
</html>
```

---

## 💥 Exploiting the Vulnerability

### Payload Used

```html
<img src=x onerror=alert('XSS')>
```

### Result

* JavaScript executes in browser
* Confirms XSS vulnerability

---

## 🔁 Stored XSS Implementation

Using `localStorage` to persist malicious input:

```js
function loadComments() {
  const saved = localStorage.getItem("comments");
  if (saved) {
    document.getElementById("comment").innerHTML = saved;
  }
}

function addComment() {
  const input = document.getElementById("userInput").value;

  let existing = localStorage.getItem("comments") || "";
  existing += "<p>" + input + "</p>";

  localStorage.setItem("comments", existing);
  loadComments();
}

loadComments();
```

### Result:

* Payload executes on every reload
* Demonstrates **Stored XSS**

---

## 🎭 Simulated Attack (Safe Demo)

A fake login overlay was injected using JavaScript to demonstrate phishing-style UI manipulation.

### Impact Demonstrated:

* UI manipulation
* User deception
* Trust exploitation

⚠️ Note: No real data was collected. This was a safe simulation.

---

## 🚨 Real-World Impact

If exploited in real applications, attackers could:

* 🔐 Steal session tokens (Account takeover)
* 🎣 Perform phishing attacks
* 📄 Access sensitive user data
* ⚙️ Perform actions on behalf of users
* 🔁 Spread malicious scripts (worm attacks)

---

## 🛡️ Fixing the Vulnerability

### ✅ Secure Code (Recommended Fix)

```js
function addComment() {
  const input = document.getElementById("userInput").value;

  const p = document.createElement("p");
  p.textContent = input; // SAFE

  document.getElementById("comment").appendChild(p);
}
```

---

## 🧼 Sanitization (Advanced)

If HTML input is required, use a sanitizer like DOMPurify:

```html
<script src="https://unpkg.com/dompurify@3.0.0/dist/purify.min.js"></script>
```

```js
const clean = DOMPurify.sanitize(input);
```

---

## 🔐 Additional Security Measures

### 1. Avoid Dangerous APIs

* ❌ `innerHTML`
* ❌ `document.write`
* ❌ `eval`

---

### 2. Use Content Security Policy (CSP)

```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; script-src 'self'; object-src 'none';">
```

---

### 3. Secure Cookies

* HTTP-only
* Secure flag
* SameSite attribute

---

### 4. Backend Validation

* Always sanitize and validate input on server-side

---

## 🧠 Key Learnings

* XSS is not limited to `<script>` tags
* Event handlers (`onerror`, `onclick`) are dangerous
* `innerHTML` is a major risk
* Defense must be applied at multiple levels

---

## 📄 Conclusion

This lab demonstrates the complete lifecycle of an XSS vulnerability:

1. Identifying the vulnerability
2. Exploiting it safely
3. Understanding real-world impact
4. Implementing secure fixes

---

## 🚀 Future Improvements

* Test XSS in modern frameworks (React, Angular)
* Learn filter bypass techniques (ethical use only)
* Practice writing professional bug bounty reports

---

## ⚖️ Disclaimer

This project is created for **educational purposes only**.
All testing was performed on a self-owned local application.

---

## 👨‍💻 Author

**Devarshi Tambulkar**

---
