# French MCP Catalog

A Docker MCP catalog providing access to French and European public data APIs.

---

## Setup

### Step 1 — Pull this catalog

```bash
docker mcp catalog import https://raw.githubusercontent.com/aureTheDev/french-mcp-catalog/main/catalog.yml
```

### Step 2 — Add servers to your profile

**Servers with no API key required:**

```bash
docker mcp server enable boamp-mcp
docker mcp server enable datagouv-mcp
docker mcp server enable recherche-entreprise-mcp
docker mcp server enable ted-mcp
```

**INSEE MCP (API key required):**

```bash
docker mcp server enable insee-mcp --env INSEE_API_KEY=votre_clé_api
```

> Get your INSEE API key at [portail-api.insee.fr](https://portail-api.insee.fr) — create an account and generate a key for the SIRENE API.

### Step 3 — Verify configuration

```bash
docker mcp server ls
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
