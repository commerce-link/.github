# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in any Commerce Link repository, please report it responsibly.

**Do not open a public GitHub issue for security vulnerabilities.**

Instead, send an email to **security@commercelink.pl** with:

- A description of the vulnerability
- Steps to reproduce the issue
- The affected repository and version (if known)
- Any potential impact assessment

## Response Timeline

- **Acknowledgment**: Within 48 hours of your report
- **Initial assessment**: Within 5 business days
- **Fix or mitigation**: Depends on severity, but we aim to resolve critical issues within 14 days

## Scope

The following areas are considered high priority:

- Authentication and authorization (OAuth2, Cognito, API keys)
- Payment processing (Stripe, PayNow integrations)
- Secret management and credential handling
- Data exposure or leakage
- Injection vulnerabilities (SQL, command, XSS)

## Supported Versions

We provide security updates for the latest released version of each repository. Older versions are not actively maintained.

## Disclosure Policy

- We will coordinate with you on disclosure timing
- We will credit you in the security advisory (unless you prefer anonymity)
- We ask that you give us reasonable time to address the issue before public disclosure

## Configuration Security

When deploying CommerceLink, ensure that:

- All `.properties` files with credentials use environment variables or AWS Secrets Manager
- Never commit real API keys, secrets, or credentials to version control
- Use the provided `.properties.example` files as templates

## Contact

- Security issues: security@commercelink.pl
- General inquiries: hello@commercelink.pl
