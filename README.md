# French MCP Catalog

A Docker MCP catalog providing access to French and European public data APIs.

---

## Setup

### Step 1 — Pull this catalog

```bash
docker mcp catalog import https://github.com/aureTheDev/french-mcp-catalog.git
```

### Step 2 — Add servers to your profile

**Servers with no API key required:**

```bash
docker mcp profile server add default --server catalog://mcp/docker-mcp-catalog/boamp-mcp
docker mcp profile server add default --server catalog://mcp/docker-mcp-catalog/datagouv-mcp
docker mcp profile server add default --server catalog://mcp/docker-mcp-catalog/recherche-entreprise-mcp
docker mcp profile server add default --server catalog://mcp/docker-mcp-catalog/ted-mcp
```

**INSEE MCP (API key required):**

```bash
docker mcp profile server add default --server catalog://mcp/french-mcp-catalog/insee-mcp
docker mcp profile config default --set insee-mcp.INSEE_API_KEY=your_api_key_here
```

> Get your INSEE API key at [portail-api.insee.fr](https://portail-api.insee.fr) — create an account and generate a key for the SIRENE API.

### Step 3 — Verify configuration

```bash
docker mcp profile config default --get-all
```

---

## Available Servers

| Server | Catalog | Description | API Key |
|--------|---------|-------------|---------|
| `boamp-mcp` | `mcp/docker-mcp-catalog` | French public procurement notices (BOAMP) | No |
| `datagouv-mcp` | `mcp/docker-mcp-catalog` | French open data platform (data.gouv.fr) | No |
| `recherche-entreprise-mcp` | `mcp/docker-mcp-catalog` | French company search | No |
| `insee-mcp` | `mcp/french-mcp-catalog` | INSEE APIs — SIRENE, geographic metadata, macroeconomic series | **Yes** |
| `ted-mcp` | `mcp/docker-mcp-catalog` | European public procurement notices (TED Europa) | No |

---

## Rate Limits

| Server | Rate Limit | Notes |
|--------|-----------|-------|
| `boamp-mcp` | ~1 000 calls/day | Anonymous access via OpenDataSoft — limits enforced via response headers |
| `datagouv-mcp` | 30 req/min | Metadata API |
| `recherche-entreprise-mcp` | 7 req/sec | May be lowered during peak load |
| `insee-mcp` | 30 req/min | SIRENE API |
| `ted-mcp` | Not published | Fair usage policy — see [ted.europa.eu](https://ted.europa.eu/en/news/fair-usage-policy-on-ted) |
