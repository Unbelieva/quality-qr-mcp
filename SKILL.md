---
name: quality-qr
description: |
  Quality QR gives AI agents trackable QR codes with scan analytics,
  dynamic URL updates, and 12 code types (URL, WiFi, vCard, PDF, etc.).
  One command connects any MCP client. Route the reader to the right
  usage path after setup.
---

# Quality QR

Quality QR lets agents create, manage, and track QR codes — saved to
the user's account with scan analytics and dynamic URL updates. No
local installation: the MCP server runs at `https://quality-qr.app/api/mcp`.

## Quick Setup

One command connects Claude Code to the MCP server:

```bash
claude mcp add quality-qr --transport http https://quality-qr.app/api/mcp \
  --header "Authorization: Bearer YOUR_API_KEY"
```

Replace `YOUR_API_KEY` with a key starting with `qr_live_`. If you don't
have one yet, see Path C below.

After setup, verify it works:

```
quality_qr_list
```

If you get back a list (even an empty one), you're connected.

## Choose Your Path

- **Need to create or manage QR codes now** → Path A (MCP tools)
- **Building an app that calls the QR API from code** → Path B (REST API)
- **Need an API key first** → Path C (account setup)

---

## Path A: Use the MCP Tools

Use this when you are an AI agent with MCP support (Claude Desktop,
Cursor, Claude Code, etc.) and need to create, update, or analyze QR
codes during this session.

### Available tools

| Tool | What it does |
|------|-------------|
| `quality_qr_create` | Create a trackable QR code (12 types). Returns an inline SVG. |
| `quality_qr_list` | List the user's QR codes with optional type filter and pagination. |
| `quality_qr_get` | Get full details of a specific QR code by ID. |
| `quality_qr_update` | Change destination URL (dynamic types), rename, or enable/disable. |
| `quality_qr_delete` | Permanently delete a QR code (frees plan quota). |
| `quality_qr_analytics` | Scan analytics: totals, daily trends, device breakdown, top countries. |

### QR code types

**Dynamic** (destination can be changed after printing):
URL, PDF, Menu, App Store, Social

**Static** (data encoded directly in the code):
WiFi, vCard, Email, Phone, SMS, Text, Event

### Default workflow

1. Use `quality_qr_create` with a `type` and `content` or `data` object.
2. The response includes an inline SVG — present it to the user.
3. For dynamic codes, use `quality_qr_update` to change the destination later.
4. Use `quality_qr_analytics` to check scan performance.

### Example prompts

- "Create a QR code for https://example.com"
- "Make a WiFi QR code for network MyWiFi with password guest123"
- "Show me analytics for my QR codes"
- "List all my QR codes"
- "Disable QR code abc123"

If the task becomes "build an app that generates QR codes," switch to
Path B.

---

## Path B: Use the REST API

Use this when you're a coding agent building an application, workflow,
or service that calls the Quality QR API from code.

### Base URL

```
https://quality-qr.app/api/v1
```

### Authentication

```
Authorization: Bearer qr_live_YOUR_KEY
```

### Create a QR code

```bash
curl -X POST https://quality-qr.app/api/v1/qr \
  -H "Authorization: Bearer qr_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "url",
    "content": "https://example.com",
    "name": "My Website",
    "isDynamic": true
  }'
```

Response:

```json
{
  "success": true,
  "type": "url",
  "dataUrl": "data:image/png;base64,...",
  "qrCode": {
    "id": "abc123",
    "name": "My Website",
    "shortCode": "xY7kL9mN",
    "shortUrl": "https://quality-qr.app/q/xY7kL9mN"
  }
}
```

### List QR codes

```bash
curl https://quality-qr.app/api/v1/qr \
  -H "Authorization: Bearer qr_live_YOUR_KEY"
```

### Supported types and required fields

| Type | Required fields | Dynamic? |
|------|----------------|----------|
| `url` | `content` (URL) | Yes |
| `pdf` | `content` (URL to PDF) | Yes |
| `menu` | `content` (URL to menu) | Yes |
| `app-store` | `content` (store URL) | Yes |
| `social` | `data.platforms` array | Yes |
| `wifi` | `data.ssid`, optional `data.password`, `data.encryption` | No |
| `vcard` | `data.firstName`, `data.lastName` | No |
| `email` | `data.to`, optional `data.subject`, `data.body` | No |
| `phone` | `data.number` | No |
| `sms` | `data.number`, optional `data.message` | No |
| `text` | `content` | No |
| `event` | `data.title`, `data.startDate` | No |

### Design options

```json
{
  "type": "url",
  "content": "https://example.com",
  "design": {
    "foregroundColor": "#1a56db",
    "backgroundColor": "#ffffff"
  }
}
```

### Error responses

| Status | Meaning |
|--------|---------|
| 400 | Invalid type or missing fields |
| 401 | Missing or invalid API key |
| 403 | Plan limit reached |
| 429 | Rate limit exceeded (check `Retry-After` header) |

If you don't have a key yet, do Path C first.

---

## Path C: Get an API Key

Use this when the human doesn't have an API key yet, or needs to create
an account.

If you already have a valid `qr_live_` key, skip this path.

**Step 1 — Ask the human to sign up or sign in:**

```
https://quality-qr.app
```

Free accounts require no credit card.

**Step 2 — Generate an API key:**

Direct the human to:

```
https://quality-qr.app/dashboard/api-keys
```

They click **Create New Key**, copy the value (starts with `qr_live_`).

**Step 3 — Save the key and continue:**

For MCP setup, go back to Quick Setup above. For app integration, set:

```bash
echo "QUALITY_QR_API_KEY=qr_live_..." >> .env
```

---

## Rate Limits

| Plan | Tool Calls / Month | Dynamic QR Codes | Analytics History | Price |
|------|-------------------|------------------|-------------------|-------|
| Free | 100 | 1 | 7 days | Free |
| Pro | 1,000 | 10 | 30 days | €10/mo |
| Business | 10,000 | 100 | 1 year | €30/mo |

MCP tool calls count against your API rate limit. Protocol messages
(`initialize`, `tools/list`) are free.

Rate-limit headers are included in every response:

- `X-RateLimit-Remaining` — calls left this billing period
- `X-RateLimit-Reset` — when the limit resets (ISO 8601)
- `Retry-After` — seconds to wait (only on 429 responses)

---

## Documentation

- **Main site:** https://quality-qr.app
- **MCP setup guide:** https://quality-qr.app/docs/mcp
- **REST API reference:** https://quality-qr.app/docs/api
- **QR code types:** https://quality-qr.app/docs/qr-types
- **Pricing:** https://quality-qr.app/pricing
