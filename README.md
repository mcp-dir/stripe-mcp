# Stripe

### Stripe para Claude, ChatGPT e agentes de IA

Pagamentos e billing no Stripe com a API REST oficial completa (api.stripe.com), cobre toda a documentação: clientes, cobranças, payment intents, assinaturas, faturas, reembolsos, disputas, produtos, preços, cupons, payment links, checkout, repasses, payouts, saldo, métodos de pagamento, tax, radar, issuing, connect e mais. Consulta e operação. Autenticação por chave secreta da conta Stripe (em Developers, API keys). Recomendamos uma chave restrita (rk_) com só os escopos necessários. Se restringir a chave por IP, libere o IP fixo de saída 3.94.44.196.

- 📊 **1 ferramenta**
- 🔒 **Somente leitura**
- 💬 **Funciona com qualquer cliente MCP**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Login via magic-link (sem senha)**

[English version](README.en.md) · [Documentação completa](docs/) · [Skill pra agentes](skills/)

---

## Instalar em 1 clique

### Claude (Web e Desktop)

A Anthropic unificou a instalação de MCPs em `claude.ai/customize/connectors`. **O mesmo link serve pra Claude Web e Claude Desktop** (basta estar logado):

[➕ Abrir no Claude e conectar](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

**Manual** (se o deeplink não abrir): [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Adicionar conector personalizado** → cole **Nome** `Stripe` e **URL** `https://api.mcp.ai/p_stripe`.

### Cursor

[➕ Instalar Stripe no Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=stripe&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9zdHJpcGUifQ==)

### VS Code (Copilot Chat)

[➕ Instalar Stripe no VS Code](vscode:mcp/install?name=stripe&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_stripe%22%7D)

### ChatGPT, Manus, OpenClaw e mais 40+ clientes

Funciona em qualquer cliente MCP que suporte **MCP over HTTP**. A URL do servidor é sempre:

```
https://api.mcp.ai/p_stripe
```

Detalhes por cliente: [INSTALL.md](INSTALL.md).

---

## Exemplos de uso

```
Qual o saldo disponível na minha conta Stripe?
Liste os últimos 10 pagamentos (payment intents)
Crie um cliente com email cliente@exemplo.com
```

---

## 1 ferramenta disponível

| Tool | Descrição |
|---|---|
| `search_tools` | Single entrypoint for MCP catalog. |

Detalhe de cada tool: [docs/ferramentas.md](docs/ferramentas.md)

---

## Preços

Grátis.

---

## Privacidade & LGPD

- **Somente leitura**, nenhuma ferramenta altera dados na origem.
- **Sub-processadores**: Stripe, o LLM host que você escolher (Claude, ChatGPT, Cursor, agente próprio). Lista completa em [docs/privacidade-lgpd.md](docs/privacidade-lgpd.md).
- Os dados retornados pelas tools são enviados ao **LLM host que você escolher**, sub-processador fora do nosso controle. Recomendamos planos com opt-out de treinamento.

---

## Perguntas frequentes

**O servidor é open source?**
O servidor é proprietário (hosted). Este repositório é o wrapper público com manifestos, docs e skills — tudo MIT.

**Posso usar com agente próprio (não Claude/Cursor)?**
Sim — qualquer cliente que suporte MCP over HTTP. URL: `https://api.mcp.ai/p_stripe`.


---

## Suporte

- 📧 [stripe@mcp.ai](mailto:stripe@mcp.ai)
- 🐛 [GitHub Issues](https://github.com/mcp-dir/stripe-mcp/issues)
- 📄 [docs/](docs/)

---

## Licença

MIT — veja [LICENSE](LICENSE). O servidor MCP em `api.mcp.ai/p_stripe` é proprietário (hosted); este repositório (manifestos, docs, skills) é MIT.
