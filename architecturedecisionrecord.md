# Architecture Decision Record (ADR) - Team 2

**ADR ID:** 0001-mightycapstone  
**Status:** Accepted  
**Date:** 2026-06-23  

---

## Context  
The Capstone project focuses on DevSecOps practices using GitHub Actions, Docker, Amazon ECR, Kubernetes Helm.  
As part of the software delivery process, the team wanted to improve application security and identify vulnerabilities before deployment.  
The project requires an automated approach that can be integrated into the CI/CD pipeline with minimal manual effort.  

---

## Options Considered  

### 1. Manual Security Review  
- **Pros:** No additional tools required. Simple to implement.  
- **Cons:** Time-consuming. Human errors may occur. Vulnerabilities may be missed. Not suitable for continuous integration.  

### 2. Automated Security Scanning in CI/CD  
- **Pros:** Automatically checks every code change. Consistent and repeatable process. Early detection of security issues. Supports DevSecOps best practices.  
- **Cons:** Longer pipeline execution time. Additional tools and configuration required.  

---

## Decision  
We chose to implement **automated security scanning in the CI/CD pipeline**.  

The pipeline includes:  
- **Secret Scanning:** Gitleaks  
- **SAST:** Semgrep  
- **SCA / Dependency Scanning:** Trivy (Filesystem mode)  
- **Container Image Scanning:** Trivy (Image mode)  
- **DAST:** OWASP ZAP  

This approach enables security checks to be performed automatically before and after deployment.  

---

## Consequences  
- Improves application security.  
- Detects vulnerabilities earlier in the development lifecycle.  
- Reduces manual security review effort.  
- Aligns with DevSecOps best practices.  