# French MCP Catalog

A Docker MCP catalog providing access to French and European public data APIs.

---

## Available Servers

| Server | Description | API Key Required |
|--------|-------------|-----------------|
| `boamp-mcp` | French public procurement notices (BOAMP) | No |
| `datagouv-mcp` | French open data platform (data.gouv.fr) | No |
| `recherche-entreprise-mcp` | French company search (recherche-entreprises.api.gouv.fr) | No |
| `insee-mcp` | INSEE APIs — SIRENE directory, geographic metadata (COG/NAF), macroeconomic series (BDM) | Optional |
| `ted-mcp` | European public procurement notices (TED Europa) | No |

---

## Setup

### Option A — Docker Desktop (recommended)

1. Open **Docker Desktop** → **MCP Toolkit** → **Catalog**
2. Click **Import catalog** and paste:
   ```
   https://raw.githubusercontent.com/aureTheDev/french-mcp-catalog/main/catalog.yml
   ```
3. Select the servers you want and add them to your active profile

### Option B — CLI (profile method)

```bash
# 1. Import the catalog
# docker mcp catalog add french-mcp ./catalog.yml
# or from URL:
docker mcp catalog add https://raw.githubusercontent.com/aureTheDev/french-mcp-catalog/main/catalog.yml

# 2. Add servers to a profile
docker mcp profile server add <profile-name> \
  --server french-mcp/boamp-mcp \
  --server french-mcp/datagouv-mcp \
  --server french-mcp/recherche-entreprise-mcp \
  --server french-mcp/ted-mcp \

# 2.2 Enable server (when no profile command)
docker mcp server enable insee-mcp
docker mcp server enable boamp-mcp
docker mcp server enable datagouv-mcp
docker mcp server enable recherche-entreprise-mcp
docker mcp server enable ted-mcp

# 3. Configure the INSEE API key in the profile
docker mcp profile config <profile-name> --set insee-mcp.insee_api_key=YOUR_KEY
```

---

## Configuring the INSEE API Key

The INSEE MCP server works without a key for most endpoints, but the **SIRENE** and **BDM** APIs require one for reliable access (rate limit goes from 30 req/min to higher tiers).

Get your key at [portail-api.insee.fr](https://portail-api.insee.fr) — free registration, generate a key for the **SIRENE** subscription.

### Method 1 — Via Docker Desktop UI

**Docker Desktop** → **MCP Toolkit** → **Catalog** → **insee-mcp** → **Configuration** → set `insee_api_key`

### Method 2 — Via CLI (profile config)

```bash
docker mcp profile config <profile-name> --set insee-mcp.insee_api_key=YOUR_KEY
```

### Method 3 — Via Claude (in conversation)

Ask Claude to configure it directly:

> "Configure the insee-mcp server with my API key: `YOUR_KEY`"

Claude will call `mcp-config-set` on your behalf.

### Method 4 — Via docker mcp config write (PowerShell)

```powershell
docker mcp config write "insee-mcp:
  insee_api_key: YOUR_KEY
"
```

---

## Rate Limits

| Server | Limit | Notes |
|--------|-------|-------|
| `boamp-mcp` | ~1 000 calls/day | Anonymous via OpenDataSoft |
| `datagouv-mcp` | 30 req/min | Metadata API |
| `recherche-entreprise-mcp` | 7 req/sec | May lower during peak load |
| `insee-mcp` | 30 req/min (no key) | Higher with API key |
| `ted-mcp` | Not published | Fair usage — [ted.europa.eu](https://ted.europa.eu/en/news/fair-usage-policy-on-ted) |
