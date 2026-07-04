# Docker Image Scanning

## Overview

Docker Image Scanning identifies known security vulnerabilities in container images before deployment.

Scanning helps detect:

- Vulnerable packages
- Outdated dependencies
- Operating system CVEs
- Security risks

---

# Why Scan Images?

Benefits:

- Detect vulnerabilities early.
- Improve application security.
- Meet compliance requirements.
- Reduce production risks.

---

# Docker Scout

Scan an image.

```bash
docker scout quickview nginx
```

Detailed scan.

```bash
docker scout cves nginx
```

---

# Trivy

Install Trivy.

```bash
trivy image nginx
```

---

# Workflow

```text
Build Image
      │
      ▼
Scan Image
      │
      ▼
Fix Vulnerabilities
      │
      ▼
Deploy
```

---

# Best Practices

- Scan every build.
- Scan before pushing images.
- Remove unnecessary packages.
- Keep base images updated.

---

# Common Mistakes

- Ignoring scan reports.
- Deploying vulnerable images.
- Using outdated base images.

---

# Summary

Image scanning helps identify vulnerabilities before deployment, improving container security and reducing operational risks.