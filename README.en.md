# Stripe

### Stripe for Claude, ChatGPT and AI agents

Payments and billing on Stripe with the full official REST API (api.stripe.com), covers the entire documentation: customers, charges, payment intents, subscriptions, invoices, refunds, disputes, products, prices, coupons, payment links, checkout, transfers, payouts, balance, payment methods, tax, radar, issuing, connect and more. Read and write. Auth via the Stripe account secret key (under Developers, API keys). We recommend a restricted key (rk_) with only the scopes you need. If you restrict the key by IP, allowlist the fixed outbound IP 3.94.44.196.

- 📊 **1 tool**
- 🔒 **Read-only**
- 💬 **Works with any MCP client**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Magic-link login (no password)**

[Portuguese version](README.md) · [Full docs (PT-BR)](docs/)

---

## One-click install

### Claude (Web and Desktop)

[➕ Open in Claude and connect](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

Manual: [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Add custom connector** → name `Stripe`, URL `https://api.mcp.ai/p_stripe`.

### Cursor

[➕ Install in Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=stripe&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9zdHJpcGUifQ==)

### VS Code (Copilot Chat)

[➕ Install in VS Code](vscode:mcp/install?name=stripe&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_stripe%22%7D)

### Any other MCP-over-HTTP client

```
https://api.mcp.ai/p_stripe
```

---

## 1 tool

| Tool | Description |
|---|---|
| `search_tools` | Single entrypoint for MCP catalog. |

---

## Pricing

Free.

---

## License

MIT — see [LICENSE](LICENSE). The MCP server at `api.mcp.ai/p_stripe` is proprietary (hosted); this repo (manifests, docs, skills) is MIT.
