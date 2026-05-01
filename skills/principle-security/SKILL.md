---
name: principle-security
description: Security design principles. Auto-load when working with authentication, authorization, input validation, secrets, trust boundaries, SSRF, or OWASP-related concerns.
---

## 🔐 Security at Design Time

Security issues are cheapest to fix before code is written.
This skill focuses on identifying risks early when designing systems.

## 🧱 Trust Boundaries & Input Validation

Every system has boundaries where untrusted data enters.

## Key rules:
Treat all external input as untrusted
Validate at boundaries, not deep inside logic
Prefer allowlists over denylists
Example
# ❌ Bad (denylist)
if "DROP" not in user_input:
    process(user_input)

# ✅ Good (allowlist)
import re

if re.match(r"^[a-zA-Z0-9_]+$", user_input):
    process(user_input)

## 🔑 Authentication vs Authorization
Authentication (AuthN)
Who are you?
Authorization (AuthZ)
What can you do?
Principle:
Always separate AuthN and AuthZ logic
Enforce least privilege

Example
# ❌ Bad
if user.is_logged_in:
    delete_all_data()

# ✅ Good
if user.is_logged_in and user.role == "admin":
    delete_all_data()

## ⚠️ Confused Deputy Problem

Occurs when a system misuses its privileges on behalf of a user.

## Prevention:
Always check who requested the action
Avoid blindly trusting internal services
🔐 Secret Handling
Rules:
Never hardcode secrets
Use environment variables
Rotate secrets regularly
Never log secrets
Example
# ❌ Bad
API_KEY = "12345"

# ✅ Good
import os
API_KEY = os.getenv("API_KEY")

## 🌐 OWASP Top 10 (Design Perspective)

Focus on preventing:

Injection attacks
Broken authentication
Sensitive data exposure
SSRF (Server-Side Request Forgery)
Example (SSRF risk)

# ❌ Bad
requests.get(user_input_url)

# ✅ Good
if user_input_url.startswith("https://trusted.com"):
    requests.get(user_input_url)

## When to Use This vs security-auditor
Use this skill when:
Designing systems
Planning APIs
Defining data flow
Use security-auditor when:
Reviewing code
Checking diffs
Finding vulnerabilities after implementation

## Key Principles Summary
Validate inputs at boundaries
Separate AuthN and AuthZ
Follow least privilege
Protect secrets
Design for security, not patch later
