# Quality QR MCP Server

Create and manage trackable QR codes through AI assistants like Claude, Cursor, and any MCP-compatible tool.

Unlike generic QR code generators, Quality QR codes are **saved to your account** with scan tracking, analytics, and the ability to update destinations without reprinting.

**Endpoint:** `https://quality-qr.app/api/mcp`

## Setup

### 1. Get an API Key

Sign up at [quality-qr.app](https://quality-qr.app) and generate an API key at **Dashboard > Settings > API Keys**.

### 2. Configure Your MCP Client

#### Claude Desktop

Add to your Claude Desktop config (`~/Library/Application Support/Claude/claude_desktop_config.json` on Mac):

```json
{
  "mcpServers": {
    "quality-qr": {
      "url": "https://quality-qr.app/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

#### Cursor

Add to your Cursor MCP settings:

```json
{
  "mcpServers": {
    "quality-qr": {
      "url": "https://quality-qr.app/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

#### Claude Code

```bash
claude mcp add quality-qr --transport http https://quality-qr.app/api/mcp \
  --header "Authorization: Bearer YOUR_API_KEY"
```

### 3. Start Using It

Ask your AI assistant:

- "Create a QR code for https://example.com"
- "Make a WiFi QR code for network MyWiFi with password guest123"
- "Show me the analytics for my QR codes"
- "List all my QR codes"
- "Disable QR code abc123"

## Available Tools

| Tool | Description |
|------|-------------|
| `quality_qr_create` | Create a trackable QR code. 12 types: URL, PDF, Menu, App Store, Social, WiFi, vCard, Email, Phone, SMS, Text, Event. Returns an inline SVG. |
| `quality_qr_list` | List your QR codes with optional type filter and pagination. |
| `quality_qr_get` | Get full details of a QR code by ID. |
| `quality_qr_update` | Update destination URL (dynamic types), rename, or enable/disable. |
| `quality_qr_delete` | Permanently delete a QR code. Frees plan quota. |
| `quality_qr_analytics` | Scan analytics: totals, daily trends, device breakdown, top countries. |

## QR Code Types

**Dynamic (URL can be changed after printing):** URL, PDF, Menu, App Store, Social

**Static (data encoded directly):** WiFi, vCard, Email, Phone, SMS, Text, Event

## Rate Limits

MCP tool calls count against your API rate limit. Protocol messages (`initialize`, `tools/list`) are free.

| Plan | Tool Calls / Month | Analytics History | Price |
|------|-------------------|-------------------|-------|
| Free | 100 | 7 days | Free |
| Pro | 1,000 | 30 days | €10/mo |
| Business | 10,000 | 1 year | €30/mo |

## Security

- All requests authenticated via API key (Bearer token)
- Keys can be revoked instantly from the dashboard
- QR codes are scoped to your account (no cross-user access)
- No admin, billing, or team management operations exposed
- GDPR and CCPA compliant, hosted on Cloudflare's global edge network

## Links

- [Quality QR](https://quality-qr.app)
- [Agent Onboarding (SKILL.md)](https://quality-qr.app/agent-onboarding/SKILL.md) — step-by-step instructions for AI agents
- [MCP Documentation](https://quality-qr.app/docs/mcp)
- [API Documentation](https://quality-qr.app/docs/api)
- [Pricing](https://quality-qr.app/pricing)

## Protocol

This is a remote MCP server using Streamable HTTP transport (JSON-RPC 2.0 over HTTP POST). No local installation required. Compatible with MCP protocol version `2025-03-26`.
