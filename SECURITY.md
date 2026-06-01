# Security Policy

Quality QR takes security seriously. This document covers how to report
vulnerabilities in the Quality QR MCP server and the hosted service it
talks to (`https://quality-qr.app`).

For end-user guidance on protecting your own API key, see the **Security**
section of [README.md](README.md).

## Reporting a Vulnerability

**Please do not open a public GitHub issue for security reports.**

Email: **security@quality-qr.app**

Encrypted reports welcome — request our PGP key in your first message
and we will reply with it.

Include in your report:

- A description of the issue and its impact
- Steps to reproduce (proof-of-concept, request/response captures, or a
  short script)
- Affected endpoint or tool (`quality_qr_*`)
- Your environment (MCP client, OS, key prefix — never the full key)
- Whether the issue is already public

## Response SLA

| Stage | Target |
|---|---|
| Acknowledgement of report | 2 business days |
| Initial triage + severity assessment | 5 business days |
| Status update cadence during investigation | At least every 7 days |
| Fix for **critical** issues (auth bypass, account takeover, data exposure) | 7 days |
| Fix for **high** issues | 30 days |
| Fix for **medium/low** issues | Next scheduled release |

If you do not receive an acknowledgement within 2 business days, please
re-send and CC `support@quality-qr.app`.

## Scope

**In scope:**

- The MCP server at `https://quality-qr.app/api/mcp`
- The REST API at `https://quality-qr.app/api/v1`
- The web application at `https://quality-qr.app`
- The short-link redirector at `https://quality-qr.app/q/*`
- Authentication, authorization, and rate-limiting behavior
- Tenant isolation between accounts
- Manifests in this repository (`server.json`, `smithery.yaml`)

**Out of scope:**

- Reports requiring physical access to a user's device
- Social engineering of Quality QR staff or customers
- Denial-of-service via traffic volume
- Findings from automated scanners without a working proof-of-concept
- Vulnerabilities in third-party MCP clients (report to the client
  vendor)
- Best-practice deviations without a demonstrated security impact
  (missing security headers on non-sensitive endpoints, etc.)

## Safe Harbor

We will not pursue legal action against researchers who:

- Make a good-faith effort to comply with this policy
- Avoid privacy violations, data destruction, and service degradation
- Test only against accounts they own or have explicit permission to test
- Give us reasonable time to remediate before any public disclosure
  (90 days from acknowledgement, or sooner if we agree)

## Disclosure

We coordinate disclosure with reporters. By default we publish a
post-fix advisory in [CHANGELOG.md](CHANGELOG.md) and credit the
reporter (with their permission). If you prefer to remain anonymous,
say so in your report.

## Supported Versions

Only the currently deployed version of the hosted service is supported.
The MCP server is continuously deployed; there are no parallel versions
to back-port fixes to.

The repository version (see [VERSION](VERSION)) tracks the manifests
and documentation, not the server runtime.
