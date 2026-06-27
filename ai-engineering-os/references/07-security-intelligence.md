# Security Intelligence

## Purpose
Comprehensive security framework covering threat modeling, secure coding, vulnerability assessment, and compliance requirements.

## Security Principles

1. **Defense in depth** - Multiple security layers
2. **Least privilege** - Minimum access necessary
3. **Fail secure** - Default to secure state
4. **Never trust input** - Validate everything
5. **Security by design** - Not bolted on
6. **Assume breach** - Plan for compromise
7. **Minimize attack surface** - Reduce exposure

## Threat Modeling

### STRIDE Framework

| Threat | Description | Mitigation |
|--------|-------------|------------|
| **Spoofing** | Impersonating identity | Authentication, MFA |
| **Tampering** | Modifying data/code | Integrity checks, signing |
| **Repudiation** | Denying actions | Logging, audit trails |
| **Information Disclosure** | Exposing data | Encryption, access control |
| **Denial of Service** | Disrupting availability | Rate limiting, scaling |
| **Elevation of Privilege** | Gaining unauthorized access | Authorization, sandboxing |

### Attack Surface Analysis

```
ANALYZE_ATTACK_SURFACE(system):
  Entry Points:
    - API endpoints
    - File uploads
    - User input fields
    - External integrations
    - Admin interfaces
    - Debug endpoints
  
  Data at Rest:
    - Databases
    - File storage
    - Backups
    - Logs
  
  Data in Transit:
    - Client-server
    - Service-to-service
    - External APIs
    - Webhooks
```

## Secure Coding Checklist

### Input Validation
- [ ] All inputs validated (size, type, format, range)
- [ ] Whitelist validation preferred over blacklist
- [ ] File uploads: type, size, content validation
- [ ] Path traversal prevented
- [ ] Command injection prevented
- [ ] SQL injection prevented (parameterized queries)
- [ ] NoSQL injection prevented
- [ ] XML external entity (XXE) attacks prevented

### Authentication
- [ ] Strong password policy enforced
- [ ] Multi-factor authentication available
- [ ] Session management secure (httponly, secure, samesite)
- [ ] Token expiration appropriate
- [ ] Rate limiting on auth endpoints
- [ ] Account lockout prevents brute force
- [ ] Secure password reset flow

### Authorization
- [ ] Principle of least privilege applied
- [ ] Role-based access control (RBAC) implemented
- [ ] Resource-level authorization checked
- [ ] Horizontal access control enforced (user A can't see user B's data)
- [ ] Vertical access control enforced (non-admin can't access admin)
- [ ] Access decisions centralized, not duplicated

### Data Protection
- [ ] Sensitive data encrypted at rest (AES-256)
- [ ] Data encrypted in transit (TLS 1.3)
- [ ] Key management separate from data
- [ ] PII minimized and pseudonymized where possible
- [ ] Secure deletion implemented
- [ ] Backup encryption verified

### Output Encoding
- [ ] HTML output encoded (XSS prevention)
- [ ] JavaScript context encoding
- [ ] URL encoding for redirects
- [ ] SQL encoding if parameterized queries not possible
- [ ] Content-Type headers set correctly
- [ ] X-Content-Type-Options: nosniff

### Dependency Security
- [ ] Dependencies scanned for vulnerabilities
- [ ] Minimal dependencies principle
- [ ] Trusted sources only
- [ ] Version pinning with update process
- [ ] License compliance verified

## Vulnerability Categories

### OWASP Top 10 (2021)

1. **Broken Access Control** - Verify every access, deny by default
2. **Cryptographic Failures** - Encrypt data, manage keys
3. **Injection** - Parameterized queries, validate input
4. **Insecure Design** - Threat modeling, secure patterns
5. **Security Misconfiguration** - Harden defaults, minimal features
6. **Vulnerable Components** - Inventory, scan, update
7. **Auth Failures** - Strong auth, session management
8. **Software Integrity** - Verify dependencies, signatures
9. **Logging Failures** - Log security events, detect breaches
10. **SSRF** - Validate URLs, network segmentation

### API Security

```json
{
  "api_security": {
    "authentication": ["OAuth2", "JWT", "API Keys"],
    "authorization": ["RBAC", "ABAC", " scopes"],
    "input_validation": ["schema_validation", "type_checking", "rate_limiting"],
    "output_security": ["data_filtering", "error_sanitization", "HATEOAS"],
    "transport": ["TLS1.3", "certificate_pinning", "HSTS"],
    "monitoring": ["access_logging", "anomaly_detection", "WAF"]
  }
}
```

## Compliance Frameworks

### SOC 2
- Security controls documented
- Access controls implemented
- Change management process
- Monitoring and alerting
- Incident response plan

### GDPR (if handling EU data)
- Data processing agreements
- Consent management
- Right to erasure
- Data portability
- Privacy by design

### HIPAA (if healthcare)
- PHI encryption
- Access logging
- Audit controls
- Business associate agreements

## Security Headers Checklist

```
REQUIRED_HEADERS:
  Strict-Transport-Security: max-age=31536000; includeSubDomains
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  Content-Security-Policy: [strict policy]
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: [minimal permissions]
```

## Incident Response

```
RESPONSE_PLAN:
  1. Detect - Monitoring, alerting, anomaly detection
  2. Triage - Severity assessment, scope determination
  3. Contain - Isolate affected systems
  4. Eradicate - Remove threat, patch vulnerability
  5. Recover - Restore systems, verify integrity
  6. Learn - Post-incident review, improve controls
```

## Security Review Template

```markdown
## Security Review: [Feature/System]

### Threat Model
- Entry points: [list]
- Data assets: [list]
- Trust boundaries: [list]

### STRIDE Analysis
| Threat | Risk | Mitigation | Status |
|--------|------|------------|--------|
| ...    | ...  | ...        | ...    |

### Secure Coding Checklist
- [ ] Input validation
- [ ] Authentication
- [ ] Authorization
- [ ] Output encoding
- [ ] Error handling
- [ ] Logging
- [ ] Dependencies

### Compliance
- [ ] SOC 2 requirements
- [ ] GDPR requirements (if applicable)
- [ ] Industry-specific requirements

### Outstanding Risks
| Risk | Severity | Mitigation Plan |
|------|----------|----------------|
| ...  | ...      | ...            |
```
