# XO Protocol

**The Trust Layer for AI-Powered Human Connection.**

XO Protocol exposes [XO](https://xoxo.space)'s social infrastructure — identity verification, AI-powered matching, reputation scores, and social signals — as developer-friendly REST endpoints.

> **Status:** Public Beta — free to use, rate limits apply.

## Quick Links

- [Landing Page](https://xoxo.space/en/protocol)
- [API Docs (Swagger)](https://protocol.xoxo.space/protocol/v1/docs)
- [OpenAPI Spec](./openapi.yaml)

---

## Endpoints

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/protocol/v1/auth/token` | POST | API Key | Exchange Firebase token or auth code for JWT |
| `/protocol/v1/auth/authorize` | POST | Public | Generate OAuth authorization code |
| `/protocol/v1/identity/verify` | GET | API Key + JWT | User's SBT verification, trust score, attestations |
| `/protocol/v1/connections/search` | GET | API Key + JWT | AI-powered connection recommendations |
| `/protocol/v1/reputation/{token}` | GET | API Key + JWT | Aura league tier and reputation score |
| `/protocol/v1/social-signals/{token}` | GET | API Key + JWT | Conversation quality and communication style |

---

## Authentication

XO Protocol supports two authentication flows:

### 1. Direct Authentication (first-party apps)

```bash
curl -X POST https://protocol.xoxo.space/protocol/v1/auth/token \
  -H "X-API-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"grant_type": "firebase", "assertion": "<firebase_id_token>"}'
```

### 2. OAuth 2.0 Authorization Code Flow (third-party apps)

```
1. Redirect user to:
   https://xoxo.space/en/oauth/authorize
     ?client_id=your_client_id
     &redirect_uri=https://yourapp.com/callback
     &scope=identity,connections
     &state=random123
     &response_type=code

2. User authenticates with Google and approves scopes

3. User is redirected to:
   https://yourapp.com/callback?code=AUTH_CODE&state=random123

4. Exchange code for token:
   POST /protocol/v1/auth/token
   {
     "grant_type": "authorization_code",
     "code": "AUTH_CODE",
     "client_id": "your_client_id",
     "client_secret": "your_secret",
     "redirect_uri": "https://yourapp.com/callback"
   }
```

**PKCE** is supported for public clients (SPAs) — use `code_challenge` + `code_verifier` instead of `client_secret`.

---

## Scopes

| Scope | Endpoint | Description |
|-------|----------|-------------|
| `identity` | `/identity/verify` | View SBT verification, trust score, attestations |
| `connections` | `/connections/search` | Search AI-recommended connections |
| `reputation` | `/reputation/{token}` | View Aura reputation score and league tier |
| `social_signals` | `/social-signals/{token}` | View conversation quality and communication style |

---

## Privacy

- **Ephemeral IDs**: Real user IDs are never exposed. Connections return `tmp_id` tokens (24h TTL, per-API-key scoped).
- **User-Authorized**: All data access requires explicit OAuth consent.
- **Scoped Tokens**: Each JWT is limited to the approved scopes.
- **Audit Logging**: All API calls are logged with hashed identifiers.

---

## Examples

| Example | Description |
|---------|-------------|
| [quickstart.js](./examples/quickstart.js) | Basic OAuth flow + API call |
| [mcp-server.js](./examples/mcp-server.js) | MCP server for Claude Desktop |

---

## Rate Limits

| Tier | Rate Limit | Price |
|------|-----------|-------|
| Free (Beta) | 100 req/min | $0 |
| Pro | 1,000 req/min | Coming soon |
| Enterprise | 10,000 req/min | Contact us |

---

## Errors

All errors follow [RFC 7807 Problem Details](https://tools.ietf.org/html/rfc7807):

```json
{
  "type": "urn:xo:error:api_key_required",
  "status": 400,
  "title": "Api Key Required",
  "detail": "Missing X-API-Key header"
}
```

---

## Getting an API Key

XO Protocol is currently in invite-only beta. Contact us at [protocol@xoxo.space](mailto:protocol@xoxo.space) to request access.

---

## Links

- Website: [xoxo.space](https://xoxo.space)
- Protocol Landing: [xoxo.space/protocol](https://xoxo.space/en/protocol)
- API Docs: [protocol.xoxo.space/protocol/v1/docs](https://protocol.xoxo.space/protocol/v1/docs)

---

**Note:** Only existing XO users can authenticate. New accounts must be created via the [XO App](https://xoxo.space).
