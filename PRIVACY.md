# Privacy

This document describes how the Quality QR MCP server processes data.
For the full service privacy policy, see
<https://quality-qr.app/privacy>.

## What the MCP server processes

When your MCP client calls a `quality_qr_*` tool, the server receives:

- **Your API key** (Bearer token) — used to authenticate the request
  and identify your account.
- **Tool arguments** — for `quality_qr_create`, this is the QR code
  destination URL or static payload (WiFi credentials, vCard fields,
  etc.) and any design options you provide.
- **Standard HTTP metadata** — IP address, user agent, request
  timestamp. Used for rate limiting, abuse detection, and operational
  logging.

The server does **not** receive any other context from your MCP
client — no conversation history, no files, no environment variables
beyond the request itself.

## What gets stored

| Data | Where | Retention |
|---|---|---|
| QR codes you create | Your account | Until you delete them |
| Scan analytics (timestamp, country, device class) | Your account | Per plan: 7 days (Free) / 30 days (Pro) / 1 year (Business) |
| API request logs (IP, endpoint, status, timestamp) | Operational logs | 30 days |
| API keys | Hashed in our database | Until you revoke them |

We do **not** store the contents of static QR codes' encoded payloads
beyond what is needed to render and serve the code (e.g., WiFi
passwords are stored so the QR can be re-rendered; they are encrypted
at rest and never logged).

We do **not** store individual scan IP addresses long-term. Scan
analytics are aggregated to country-level on ingest.

## What we don't do

- We do not sell or share your data with advertisers.
- We do not use your QR content or scan data to train any model.
- We do not call third-party services with your tool arguments. The
  MCP server is self-contained on Cloudflare's edge.
- We do not log your API key in plaintext.

## Third parties

The service runs on **Cloudflare** (compute, storage, CDN). Cloudflare
acts as our data processor under a standard DPA. See
<https://www.cloudflare.com/privacypolicy/>.

Payment processing (Pro/Business plans) is handled by **Stripe**. The
MCP server itself does not handle payment data.

## Your rights

You can:

- **Export your data** — Dashboard > Settings > Export.
- **Delete your account** — Dashboard > Settings > Delete Account.
  Removes all QR codes, scan history, and API keys within 30 days.
- **Revoke a key** — Dashboard > Settings > API Keys. Takes effect
  immediately.
- **Request access, correction, deletion, or portability** under GDPR
  (EU/UK) or CCPA (California) — email `privacy@quality-qr.app`. We
  respond within 30 days.

## Data residency

Data is stored in Cloudflare's global network with primary regions in
the EU and US. We do not guarantee single-region residency on Free or
Pro plans. Business-plan customers can request EU-only storage.

## Children

Quality QR is not directed at children under 16. We do not knowingly
collect data from children. If you believe a child has created an
account, email `privacy@quality-qr.app` and we will remove it.

## Changes

Material changes to this document will be announced in
[CHANGELOG.md](CHANGELOG.md) and via email to account holders at least
30 days before taking effect.

Last updated: 2026-05-27
