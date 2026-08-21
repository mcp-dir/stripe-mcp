---
name: stripe-mcp
description: Skill da REST API do Stripe na MCP.AI: 590 endpoints em /api/stripe. Pagamentos e billing no Stripe com a API REST oficial completa (api.stripe.com), cobre toda a documentação: clientes, cobranças, payment intents, assinaturas, faturas, reembolsos, disputas, produtos, preços, cupons, payment links, checkout, repasses, payouts, saldo, métodos de pagamento, tax, radar, issuing, connect e mais. Consulta e operação. Autenticação por chave secreta da conta Stripe (em Developers, API keys). Recomendamos uma chave restrita (rk_) com só os escopos necessários. Se restringir a chave por IP, libere o IP fixo de saída 3.94.44.196. Autentique com workspace API key (sk_live) gerada em app.mcp.ai/settings/api-keys. Use quando o usuário pedir algo coberto pelos endpoints.
---

# Stripe — REST API skill

Você tem acesso à **Stripe** REST API na MCP.AI.

> Pagamentos e billing no Stripe com a API REST oficial completa (api.stripe.com), cobre toda a documentação: clientes, cobranças, payment intents, assinaturas, faturas, reembolsos, disputas, produtos, preços, cupons, payment links, checkout, repasses, payouts, saldo, métodos de pagamento, tax, radar, issuing, connect e mais. Consulta e operação. Autenticação por chave secreta da conta Stripe (em Developers, API keys). Recomendamos uma chave restrita (rk_) com só os escopos necessários. Se restringir a chave por IP, libere o IP fixo de saída 3.94.44.196.

## Base URL

```
https://api.mcp.ai/api/stripe
```

Todo endpoint é um **POST** na Base URL + o path abaixo. Os parâmetros vão no corpo JSON.

## Autenticação

Inclua em toda request:

```
Authorization: Bearer sk_live_...
Content-Type: application/json
```

> Gere sua chave em **https://app.mcp.ai/settings/api-keys** (workspace API key `sk_live_…`, não expira, revogável). Uma única chave serve pra todos os seus MCPs.

## Formato de resposta

```json
{ "ok": true, "tool": "<tool_id>", "result": <payload> }
```

## Exemplo cURL

```bash
curl -X POST https://api.mcp.ai/api/stripe/account/links/create \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -d '{}'
```

## Reportar problemas

Se um endpoint retornar erro, vazio ou dado inesperado, reporte (não desista calado): **POST /api/stripe/report** com `{ "message": "...", "context"?: "...", "conversation"?: [...] }`. Isso notifica o time da MCP.AI.

## Endpoints (590)

#### `stripe_account_links_create`

Create an account link (POST /v1/account_links). _(POST /api/stripe/account/links/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_account_list`

Retrieve account (GET /v1/account). _(POST /api/stripe/account/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_account_sessions_create`

Create an Account Session (POST /v1/account_sessions). _(POST /api/stripe/account/sessions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_bank_accounts_create`

Create an external account (POST /v1/accounts/{account}/bank_accounts). _(POST /api/stripe/accounts/bank/accounts/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_bank_accounts_delete`

Delete an external account (DELETE /v1/accounts/{account}/bank_accounts/{id}). _(POST /api/stripe/accounts/bank/accounts/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_accounts_bank_accounts_get`

Retrieve an external account (GET /v1/accounts/{account}/bank_accounts/{id}). _(POST /api/stripe/accounts/bank/accounts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_accounts_bank_accounts_update`

Update a bank account (POST /v1/accounts/{account}/bank_accounts/{id}). _(POST /api/stripe/accounts/bank/accounts/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_accounts_capabilities_get`

Retrieve an Account Capability (GET /v1/accounts/{account}/capabilities/{capability}). _(POST /api/stripe/accounts/capabilities/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `capability` | string | Sim | Path param "capability" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_capabilities_list`

List all account capabilities (GET /v1/accounts/{account}/capabilities). _(POST /api/stripe/accounts/capabilities/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_capabilities_update`

Update an Account Capability (POST /v1/accounts/{account}/capabilities/{capability}). _(POST /api/stripe/accounts/capabilities/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `capability` | string | Sim | Path param "capability" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_create`

Create an account (POST /v1/accounts). _(POST /api/stripe/accounts/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_delete`

Delete an account (DELETE /v1/accounts/{account}). _(POST /api/stripe/accounts/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_external_accounts_create`

Create an external account (POST /v1/accounts/{account}/external_accounts). _(POST /api/stripe/accounts/external/accounts/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_external_accounts_delete`

Delete an external account (DELETE /v1/accounts/{account}/external_accounts/{id}). _(POST /api/stripe/accounts/external/accounts/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_accounts_external_accounts_get`

Retrieve an external account (GET /v1/accounts/{account}/external_accounts/{id}). _(POST /api/stripe/accounts/external/accounts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_accounts_external_accounts_list`

List all external accounts (GET /v1/accounts/{account}/external_accounts). _(POST /api/stripe/accounts/external/accounts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_external_accounts_update`

Update a bank account (POST /v1/accounts/{account}/external_accounts/{id}). _(POST /api/stripe/accounts/external/accounts/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_accounts_get`

Retrieve account (GET /v1/accounts/{account}). _(POST /api/stripe/accounts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_list`

List all connected accounts (GET /v1/accounts). _(POST /api/stripe/accounts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_login_links_create`

Create a login link (POST /v1/accounts/{account}/login_links). _(POST /api/stripe/accounts/login/links/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_people_create`

Create a person (POST /v1/accounts/{account}/people). _(POST /api/stripe/accounts/people/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_people_delete`

Delete a person (DELETE /v1/accounts/{account}/people/{person}). _(POST /api/stripe/accounts/people/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `person` | string | Sim | Path param "person" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_people_get`

Retrieve a person (GET /v1/accounts/{account}/people/{person}). _(POST /api/stripe/accounts/people/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `person` | string | Sim | Path param "person" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_people_list`

List all persons (GET /v1/accounts/{account}/people). _(POST /api/stripe/accounts/people/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_people_update`

Update a person (POST /v1/accounts/{account}/people/{person}). _(POST /api/stripe/accounts/people/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `person` | string | Sim | Path param "person" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_persons_create`

Create a person (POST /v1/accounts/{account}/persons). _(POST /api/stripe/accounts/persons/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_persons_delete`

Delete a person (DELETE /v1/accounts/{account}/persons/{person}). _(POST /api/stripe/accounts/persons/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `person` | string | Sim | Path param "person" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_persons_get`

Retrieve a person (GET /v1/accounts/{account}/persons/{person}). _(POST /api/stripe/accounts/persons/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `person` | string | Sim | Path param "person" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_persons_list`

List all persons (GET /v1/accounts/{account}/persons). _(POST /api/stripe/accounts/persons/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_accounts_persons_update`

Update a person (POST /v1/accounts/{account}/persons/{person}). _(POST /api/stripe/accounts/persons/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `person` | string | Sim | Path param "person" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_reject_create`

Reject an account (POST /v1/accounts/{account}/reject). _(POST /api/stripe/accounts/reject/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_unreject_create`

Unreject an account (POST /v1/accounts/{account}/unreject). _(POST /api/stripe/accounts/unreject/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_accounts_update`

Update an account (POST /v1/accounts/{account}). _(POST /api/stripe/accounts/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_apple_pay_domains_create`

PostApplePayDomains (POST /v1/apple_pay/domains). _(POST /api/stripe/apple/pay/domains/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_apple_pay_domains_delete`

DeleteApplePayDomainsDomain (DELETE /v1/apple_pay/domains/{domain}). _(POST /api/stripe/apple/pay/domains/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `domain` | string | Sim | Path param "domain" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_apple_pay_domains_get`

GetApplePayDomainsDomain (GET /v1/apple_pay/domains/{domain}). _(POST /api/stripe/apple/pay/domains/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `domain` | string | Sim | Path param "domain" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_apple_pay_domains_list`

GetApplePayDomains (GET /v1/apple_pay/domains). _(POST /api/stripe/apple/pay/domains/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_application_fees_get`

Retrieve an application fee (GET /v1/application_fees/{id}). _(POST /api/stripe/application/fees/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_application_fees_list`

List all application fees (GET /v1/application_fees). _(POST /api/stripe/application/fees/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_application_fees_refund_create`

PostApplicationFeesIdRefund (POST /v1/application_fees/{id}/refund). _(POST /api/stripe/application/fees/refund/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_application_fees_refunds_create`

Create an application fee refund (POST /v1/application_fees/{id}/refunds). _(POST /api/stripe/application/fees/refunds/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_application_fees_refunds_get`

Retrieve an application fee refund (GET /v1/application_fees/{fee}/refunds/{id}). _(POST /api/stripe/application/fees/refunds/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `fee` | string | Sim | Path param "fee" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_application_fees_refunds_list`

List all application fee refunds (GET /v1/application_fees/{id}/refunds). _(POST /api/stripe/application/fees/refunds/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_application_fees_refunds_update`

Update an application fee refund (POST /v1/application_fees/{fee}/refunds/{id}). _(POST /api/stripe/application/fees/refunds/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `fee` | string | Sim | Path param "fee" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_apps_secrets_create`

Set a Secret (POST /v1/apps/secrets). _(POST /api/stripe/apps/secrets/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_apps_secrets_delete_create`

Delete a Secret (POST /v1/apps/secrets/delete). _(POST /api/stripe/apps/secrets/delete/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_apps_secrets_find_list`

Find a Secret (GET /v1/apps/secrets/find). _(POST /api/stripe/apps/secrets/find/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_apps_secrets_list`

List secrets (GET /v1/apps/secrets). _(POST /api/stripe/apps/secrets/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_balance_history_get`

Retrieve a balance transaction (GET /v1/balance/history/{id}). _(POST /api/stripe/balance/history/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_balance_history_list`

List all balance transactions (GET /v1/balance/history). _(POST /api/stripe/balance/history/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_balance_list`

Retrieve balance (GET /v1/balance). _(POST /api/stripe/balance/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_balance_settings_create`

Update balance settings (POST /v1/balance_settings). _(POST /api/stripe/balance/settings/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_balance_settings_list`

Retrieve balance settings (GET /v1/balance_settings). _(POST /api/stripe/balance/settings/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_balance_transactions_get`

Retrieve a balance transaction (GET /v1/balance_transactions/{id}). _(POST /api/stripe/balance/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_balance_transactions_list`

List all balance transactions (GET /v1/balance_transactions). _(POST /api/stripe/balance/transactions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_billing_alerts_activate_create`

Activate a billing alert (POST /v1/billing/alerts/{id}/activate). _(POST /api/stripe/billing/alerts/activate/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_alerts_archive_create`

Archive a billing alert (POST /v1/billing/alerts/{id}/archive). _(POST /api/stripe/billing/alerts/archive/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_alerts_create`

Create a billing alert (POST /v1/billing/alerts). _(POST /api/stripe/billing/alerts/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_billing_alerts_deactivate_create`

Deactivate a billing alert (POST /v1/billing/alerts/{id}/deactivate). _(POST /api/stripe/billing/alerts/deactivate/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_alerts_get`

Retrieve a billing alert (GET /v1/billing/alerts/{id}). _(POST /api/stripe/billing/alerts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_alerts_list`

List billing alerts (GET /v1/billing/alerts). _(POST /api/stripe/billing/alerts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_billing_credit_balance_summary_list`

Retrieve the credit balance summary for a customer (GET /v1/billing/credit_balance_summary). _(POST /api/stripe/billing/credit/balance/summary/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_billing_credit_balance_transactions_get`

Retrieve a credit balance transaction (GET /v1/billing/credit_balance_transactions/{id}). _(POST /api/stripe/billing/credit/balance/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_credit_balance_transactions_list`

List credit balance transactions (GET /v1/billing/credit_balance_transactions). _(POST /api/stripe/billing/credit/balance/transactions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_billing_credit_grants_create`

Create a credit grant (POST /v1/billing/credit_grants). _(POST /api/stripe/billing/credit/grants/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_billing_credit_grants_expire_create`

Expire a credit grant (POST /v1/billing/credit_grants/{id}/expire). _(POST /api/stripe/billing/credit/grants/expire/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_credit_grants_get`

Retrieve a credit grant (GET /v1/billing/credit_grants/{id}). _(POST /api/stripe/billing/credit/grants/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_credit_grants_list`

List credit grants (GET /v1/billing/credit_grants). _(POST /api/stripe/billing/credit/grants/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_billing_credit_grants_update`

Update a credit grant (POST /v1/billing/credit_grants/{id}). _(POST /api/stripe/billing/credit/grants/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_credit_grants_void_create`

Void a credit grant (POST /v1/billing/credit_grants/{id}/void). _(POST /api/stripe/billing/credit/grants/void/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_meter_event_adjustments_create`

Create a billing meter event adjustment (POST /v1/billing/meter_event_adjustments). _(POST /api/stripe/billing/meter/event/adjustments/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_billing_meter_events_create`

Create a billing meter event (POST /v1/billing/meter_events). _(POST /api/stripe/billing/meter/events/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_billing_meters_create`

Create a billing meter (POST /v1/billing/meters). _(POST /api/stripe/billing/meters/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_billing_meters_deactivate_create`

Deactivate a billing meter (POST /v1/billing/meters/{id}/deactivate). _(POST /api/stripe/billing/meters/deactivate/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_meters_event_summaries_list`

List billing meter event summaries (GET /v1/billing/meters/{id}/event_summaries). _(POST /api/stripe/billing/meters/event/summaries/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_meters_get`

Retrieve a billing meter (GET /v1/billing/meters/{id}). _(POST /api/stripe/billing/meters/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_meters_list`

List billing meters (GET /v1/billing/meters). _(POST /api/stripe/billing/meters/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_billing_meters_reactivate_create`

Reactivate a billing meter (POST /v1/billing/meters/{id}/reactivate). _(POST /api/stripe/billing/meters/reactivate/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_meters_update`

Update a billing meter (POST /v1/billing/meters/{id}). _(POST /api/stripe/billing/meters/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_billing_portal_configurations_create`

Create a portal configuration (POST /v1/billing_portal/configurations). _(POST /api/stripe/billing/portal/configurations/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_billing_portal_configurations_get`

Retrieve a portal configuration (GET /v1/billing_portal/configurations/{configuration}). _(POST /api/stripe/billing/portal/configurations/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `configuration` | string | Sim | Path param "configuration" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_billing_portal_configurations_list`

List portal configurations (GET /v1/billing_portal/configurations). _(POST /api/stripe/billing/portal/configurations/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_billing_portal_configurations_update`

Update a portal configuration (POST /v1/billing_portal/configurations/{configuration}). _(POST /api/stripe/billing/portal/configurations/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `configuration` | string | Sim | Path param "configuration" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_billing_portal_sessions_create`

Create a portal session (POST /v1/billing_portal/sessions). _(POST /api/stripe/billing/portal/sessions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_charges_capture_create`

Capture a charge (POST /v1/charges/{charge}/capture). _(POST /api/stripe/charges/capture/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_charges_create`

Create a charge (POST /v1/charges). _(POST /api/stripe/charges/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_charges_dispute_close_create`

PostChargesChargeDisputeClose (POST /v1/charges/{charge}/dispute/close). _(POST /api/stripe/charges/dispute/close/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_charges_dispute_create`

PostChargesChargeDispute (POST /v1/charges/{charge}/dispute). _(POST /api/stripe/charges/dispute/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_charges_dispute_list`

GetChargesChargeDispute (GET /v1/charges/{charge}/dispute). _(POST /api/stripe/charges/dispute/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_charges_get`

Retrieve a charge (GET /v1/charges/{charge}). _(POST /api/stripe/charges/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_charges_list`

List all charges (GET /v1/charges). _(POST /api/stripe/charges/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_charges_refund_create`

Create a refund (POST /v1/charges/{charge}/refund). _(POST /api/stripe/charges/refund/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_charges_refunds_create`

Create a refund (POST /v1/charges/{charge}/refunds). _(POST /api/stripe/charges/refunds/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_charges_refunds_get`

GetChargesChargeRefundsRefund (GET /v1/charges/{charge}/refunds/{refund}). _(POST /api/stripe/charges/refunds/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `refund` | string | Sim | Path param "refund" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_charges_refunds_list`

List all refunds (GET /v1/charges/{charge}/refunds). _(POST /api/stripe/charges/refunds/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_charges_refunds_update`

PostChargesChargeRefundsRefund (POST /v1/charges/{charge}/refunds/{refund}). _(POST /api/stripe/charges/refunds/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `refund` | string | Sim | Path param "refund" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_charges_search_list`

Search charges (GET /v1/charges/search). _(POST /api/stripe/charges/search/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_charges_update`

Update a charge (POST /v1/charges/{charge}). _(POST /api/stripe/charges/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `charge` | string | Sim | Path param "charge" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_checkout_sessions_create`

Create a Checkout Session (POST /v1/checkout/sessions). _(POST /api/stripe/checkout/sessions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_checkout_sessions_expire_create`

Expire a Checkout Session (POST /v1/checkout/sessions/{session}/expire). _(POST /api/stripe/checkout/sessions/expire/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_checkout_sessions_get`

Retrieve a Checkout Session (GET /v1/checkout/sessions/{session}). _(POST /api/stripe/checkout/sessions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_checkout_sessions_line_items_list`

Retrieve a Checkout Session's line items (GET /v1/checkout/sessions/{session}/line_items). _(POST /api/stripe/checkout/sessions/line/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_checkout_sessions_list`

List all Checkout Sessions (GET /v1/checkout/sessions). _(POST /api/stripe/checkout/sessions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_checkout_sessions_update`

Update a Checkout Session (POST /v1/checkout/sessions/{session}). _(POST /api/stripe/checkout/sessions/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_climate_orders_cancel_create`

Cancel an order (POST /v1/climate/orders/{order}/cancel). _(POST /api/stripe/climate/orders/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `order` | string | Sim | Path param "order" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_climate_orders_create`

Create an order (POST /v1/climate/orders). _(POST /api/stripe/climate/orders/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_climate_orders_get`

Retrieve an order (GET /v1/climate/orders/{order}). _(POST /api/stripe/climate/orders/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `order` | string | Sim | Path param "order" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_climate_orders_list`

List orders (GET /v1/climate/orders). _(POST /api/stripe/climate/orders/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_climate_orders_update`

Update an order (POST /v1/climate/orders/{order}). _(POST /api/stripe/climate/orders/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `order` | string | Sim | Path param "order" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_climate_products_get`

Retrieve a product (GET /v1/climate/products/{product}). _(POST /api/stripe/climate/products/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `product` | string | Sim | Path param "product" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_climate_products_list`

List products (GET /v1/climate/products). _(POST /api/stripe/climate/products/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_climate_suppliers_get`

Retrieve a supplier (GET /v1/climate/suppliers/{supplier}). _(POST /api/stripe/climate/suppliers/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `supplier` | string | Sim | Path param "supplier" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_climate_suppliers_list`

List suppliers (GET /v1/climate/suppliers). _(POST /api/stripe/climate/suppliers/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_confirmation_tokens_get`

Retrieve a ConfirmationToken (GET /v1/confirmation_tokens/{confirmation_token}). _(POST /api/stripe/confirmation/tokens/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `confirmation_token` | string | Sim | Path param "confirmation_token" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_country_specs_get`

Retrieve a Country Spec (GET /v1/country_specs/{country}). _(POST /api/stripe/country/specs/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `country` | string | Sim | Path param "country" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_country_specs_list`

List Country Specs (GET /v1/country_specs). _(POST /api/stripe/country/specs/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_coupons_create`

Create a coupon (POST /v1/coupons). _(POST /api/stripe/coupons/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_coupons_delete`

Delete a coupon (DELETE /v1/coupons/{coupon}). _(POST /api/stripe/coupons/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `coupon` | string | Sim | Path param "coupon" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_coupons_get`

Retrieve a coupon (GET /v1/coupons/{coupon}). _(POST /api/stripe/coupons/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `coupon` | string | Sim | Path param "coupon" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_coupons_list`

List all coupons (GET /v1/coupons). _(POST /api/stripe/coupons/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_coupons_update`

Update a coupon (POST /v1/coupons/{coupon}). _(POST /api/stripe/coupons/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `coupon` | string | Sim | Path param "coupon" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_credit_notes_create`

Create a credit note (POST /v1/credit_notes). _(POST /api/stripe/credit/notes/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_credit_notes_get`

Retrieve a credit note (GET /v1/credit_notes/{id}). _(POST /api/stripe/credit/notes/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_credit_notes_lines_list`

Retrieve a credit note's line items (GET /v1/credit_notes/{credit_note}/lines). _(POST /api/stripe/credit/notes/lines/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `credit_note` | string | Sim | Path param "credit_note" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_credit_notes_list`

List all credit notes (GET /v1/credit_notes). _(POST /api/stripe/credit/notes/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_credit_notes_preview_lines_list`

Retrieve a credit note preview's line items (GET /v1/credit_notes/preview/lines). _(POST /api/stripe/credit/notes/preview/lines/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_credit_notes_preview_list`

Preview a credit note (GET /v1/credit_notes/preview). _(POST /api/stripe/credit/notes/preview/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_credit_notes_update`

Update a credit note (POST /v1/credit_notes/{id}). _(POST /api/stripe/credit/notes/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_credit_notes_void_create`

Void a credit note (POST /v1/credit_notes/{id}/void). _(POST /api/stripe/credit/notes/void/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customer_sessions_create`

Create a Customer Session (POST /v1/customer_sessions). _(POST /api/stripe/customer/sessions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_balance_transactions_create`

Create a customer balance transaction (POST /v1/customers/{customer}/balance_transactions). _(POST /api/stripe/customers/balance/transactions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_balance_transactions_get`

Retrieve a customer balance transaction (GET /v1/customers/{customer}/balance_transactions/{transaction}). _(POST /api/stripe/customers/balance/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_balance_transactions_list`

List customer balance transactions (GET /v1/customers/{customer}/balance_transactions). _(POST /api/stripe/customers/balance/transactions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_balance_transactions_update`

Update a customer credit balance transaction (POST /v1/customers/{customer}/balance_transactions/{transaction}). _(POST /api/stripe/customers/balance/transactions/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_bank_accounts_create`

Create a card (POST /v1/customers/{customer}/bank_accounts). _(POST /api/stripe/customers/bank/accounts/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_bank_accounts_delete`

Delete a customer source (DELETE /v1/customers/{customer}/bank_accounts/{id}). _(POST /api/stripe/customers/bank/accounts/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_bank_accounts_get`

Retrieve a bank account (GET /v1/customers/{customer}/bank_accounts/{id}). _(POST /api/stripe/customers/bank/accounts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_bank_accounts_list`

List all bank accounts (GET /v1/customers/{customer}/bank_accounts). _(POST /api/stripe/customers/bank/accounts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_bank_accounts_update`

Update a card (POST /v1/customers/{customer}/bank_accounts/{id}). _(POST /api/stripe/customers/bank/accounts/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_bank_accounts_verify_create`

Verify a bank account (POST /v1/customers/{customer}/bank_accounts/{id}/verify). _(POST /api/stripe/customers/bank/accounts/verify/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_cards_create`

Create a card (POST /v1/customers/{customer}/cards). _(POST /api/stripe/customers/cards/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_cards_delete`

Delete a customer source (DELETE /v1/customers/{customer}/cards/{id}). _(POST /api/stripe/customers/cards/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_cards_get`

Retrieve a card (GET /v1/customers/{customer}/cards/{id}). _(POST /api/stripe/customers/cards/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_cards_list`

List all cards (GET /v1/customers/{customer}/cards). _(POST /api/stripe/customers/cards/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_cards_update`

Update a card (POST /v1/customers/{customer}/cards/{id}). _(POST /api/stripe/customers/cards/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_cash_balance_create`

Update a cash balance's settings (POST /v1/customers/{customer}/cash_balance). _(POST /api/stripe/customers/cash/balance/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_cash_balance_list`

Retrieve a cash balance (GET /v1/customers/{customer}/cash_balance). _(POST /api/stripe/customers/cash/balance/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_cash_balance_transactions_get`

Retrieve a cash balance transaction (GET /v1/customers/{customer}/cash_balance_transactions/{transaction}). _(POST /api/stripe/customers/cash/balance/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_cash_balance_transactions_list`

List cash balance transactions (GET /v1/customers/{customer}/cash_balance_transactions). _(POST /api/stripe/customers/cash/balance/transactions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_create`

Create a customer (POST /v1/customers). _(POST /api/stripe/customers/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_delete`

Delete a customer (DELETE /v1/customers/{customer}). _(POST /api/stripe/customers/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_discount_delete`

Delete a customer discount (DELETE /v1/customers/{customer}/discount). _(POST /api/stripe/customers/discount/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_discount_list`

GetCustomersCustomerDiscount (GET /v1/customers/{customer}/discount). _(POST /api/stripe/customers/discount/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_funding_instructions_create`

Create or retrieve funding instructions for a customer cash balance (POST /v1/customers/{customer}/funding_instructions). _(POST /api/stripe/customers/funding/instructions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_get`

Retrieve a customer (GET /v1/customers/{customer}). _(POST /api/stripe/customers/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_list`

List all customers (GET /v1/customers). _(POST /api/stripe/customers/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_payment_methods_get`

Retrieve a Customer's PaymentMethod (GET /v1/customers/{customer}/payment_methods/{payment_method}). _(POST /api/stripe/customers/payment/methods/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `payment_method` | string | Sim | Path param "payment_method" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_payment_methods_list`

List a Customer's PaymentMethods (GET /v1/customers/{customer}/payment_methods). _(POST /api/stripe/customers/payment/methods/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_search_list`

Search customers (GET /v1/customers/search). _(POST /api/stripe/customers/search/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_sources_create`

Create a card (POST /v1/customers/{customer}/sources). _(POST /api/stripe/customers/sources/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_sources_delete`

Delete a customer source (DELETE /v1/customers/{customer}/sources/{id}). _(POST /api/stripe/customers/sources/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_sources_get`

GetCustomersCustomerSourcesId (GET /v1/customers/{customer}/sources/{id}). _(POST /api/stripe/customers/sources/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_sources_list`

GetCustomersCustomerSources (GET /v1/customers/{customer}/sources). _(POST /api/stripe/customers/sources/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_sources_update`

Update a card (POST /v1/customers/{customer}/sources/{id}). _(POST /api/stripe/customers/sources/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_sources_verify_create`

Verify a bank account (POST /v1/customers/{customer}/sources/{id}/verify). _(POST /api/stripe/customers/sources/verify/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_subscriptions_create`

Create a subscription (POST /v1/customers/{customer}/subscriptions). _(POST /api/stripe/customers/subscriptions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_subscriptions_delete`

Cancel a subscription (DELETE /v1/customers/{customer}/subscriptions/{subscription_exposed_id}). _(POST /api/stripe/customers/subscriptions/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_customers_subscriptions_discount_delete`

Delete a customer discount (DELETE /v1/customers/{customer}/subscriptions/{subscription_exposed_id}/discount). _(POST /api/stripe/customers/subscriptions/discount/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_customers_subscriptions_discount_list`

GetCustomersCustomerSubscriptionsSubscriptionExposedIdDiscount (GET /v1/customers/{customer}/subscriptions/{subscription_exposed_id}/discount). _(POST /api/stripe/customers/subscriptions/discount/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_customers_subscriptions_get`

Retrieve a subscription (GET /v1/customers/{customer}/subscriptions/{subscription_exposed_id}). _(POST /api/stripe/customers/subscriptions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_customers_subscriptions_list`

List active subscriptions (GET /v1/customers/{customer}/subscriptions). _(POST /api/stripe/customers/subscriptions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_subscriptions_update`

Update a subscription on a customer (POST /v1/customers/{customer}/subscriptions/{subscription_exposed_id}). _(POST /api/stripe/customers/subscriptions/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_customers_tax_ids_create`

Create a Customer tax ID (POST /v1/customers/{customer}/tax_ids). _(POST /api/stripe/customers/tax/ids/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_customers_tax_ids_delete`

Delete a Customer tax ID (DELETE /v1/customers/{customer}/tax_ids/{id}). _(POST /api/stripe/customers/tax/ids/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_tax_ids_get`

Retrieve a Customer tax ID (GET /v1/customers/{customer}/tax_ids/{id}). _(POST /api/stripe/customers/tax/ids/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_customers_tax_ids_list`

List all Customer tax IDs (GET /v1/customers/{customer}/tax_ids). _(POST /api/stripe/customers/tax/ids/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_customers_update`

Update a customer (POST /v1/customers/{customer}). _(POST /api/stripe/customers/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_disputes_close_create`

Close a dispute (POST /v1/disputes/{dispute}/close). _(POST /api/stripe/disputes/close/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `dispute` | string | Sim | Path param "dispute" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_disputes_get`

Retrieve a dispute (GET /v1/disputes/{dispute}). _(POST /api/stripe/disputes/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `dispute` | string | Sim | Path param "dispute" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_disputes_list`

List all disputes (GET /v1/disputes). _(POST /api/stripe/disputes/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_disputes_update`

Update a dispute (POST /v1/disputes/{dispute}). _(POST /api/stripe/disputes/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `dispute` | string | Sim | Path param "dispute" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_entitlements_active_entitlements_get`

Retrieve an active entitlement (GET /v1/entitlements/active_entitlements/{id}). _(POST /api/stripe/entitlements/active/entitlements/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_entitlements_active_entitlements_list`

List all active entitlements (GET /v1/entitlements/active_entitlements). _(POST /api/stripe/entitlements/active/entitlements/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_entitlements_features_create`

Create a feature (POST /v1/entitlements/features). _(POST /api/stripe/entitlements/features/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_entitlements_features_get`

Retrieve a feature (GET /v1/entitlements/features/{id}). _(POST /api/stripe/entitlements/features/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_entitlements_features_list`

List all features (GET /v1/entitlements/features). _(POST /api/stripe/entitlements/features/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_entitlements_features_update`

Updates a feature (POST /v1/entitlements/features/{id}). _(POST /api/stripe/entitlements/features/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_ephemeral_keys_create`

Create an ephemeral key (POST /v1/ephemeral_keys). _(POST /api/stripe/ephemeral/keys/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_ephemeral_keys_delete`

Immediately invalidate an ephemeral key (DELETE /v1/ephemeral_keys/{key}). _(POST /api/stripe/ephemeral/keys/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `key` | string | Sim | Path param "key" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_events_get`

Retrieve an event (GET /v1/events/{id}). _(POST /api/stripe/events/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_events_list`

List all events (GET /v1/events). _(POST /api/stripe/events/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_exchange_rates_get`

Retrieve an exchange rate (GET /v1/exchange_rates/{rate_id}). _(POST /api/stripe/exchange/rates/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `rate_id` | string | Sim | Path param "rate_id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `rate_ids` | string[] | Não | Bulk mode: multiple values for rate_id |

#### `stripe_exchange_rates_list`

List all exchange rates (GET /v1/exchange_rates). _(POST /api/stripe/exchange/rates/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_external_accounts_update`

Update a bank account (POST /v1/external_accounts/{id}). _(POST /api/stripe/external/accounts/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_file_links_create`

Create a file link (POST /v1/file_links). _(POST /api/stripe/file/links/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_file_links_get`

Retrieve a file link (GET /v1/file_links/{link}). _(POST /api/stripe/file/links/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `link` | string | Sim | Path param "link" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_file_links_list`

List all file links (GET /v1/file_links). _(POST /api/stripe/file/links/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_file_links_update`

Update a file link (POST /v1/file_links/{link}). _(POST /api/stripe/file/links/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `link` | string | Sim | Path param "link" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_files_create`

Create a file (POST /v1/files). _(POST /api/stripe/files/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_files_get`

Retrieve a file (GET /v1/files/{file}). _(POST /api/stripe/files/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `file` | string | Sim | Path param "file" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_files_list`

List all files (GET /v1/files). _(POST /api/stripe/files/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_financial_connections_accounts_disconnect_create`

Disconnect an Account (POST /v1/financial_connections/accounts/{account}/disconnect). _(POST /api/stripe/financial/connections/accounts/disconnect/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_financial_connections_accounts_get`

Retrieve an Account (GET /v1/financial_connections/accounts/{account}). _(POST /api/stripe/financial/connections/accounts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_financial_connections_accounts_list`

List Accounts (GET /v1/financial_connections/accounts). _(POST /api/stripe/financial/connections/accounts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_financial_connections_accounts_owners_list`

List Account Owners (GET /v1/financial_connections/accounts/{account}/owners). _(POST /api/stripe/financial/connections/accounts/owners/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_financial_connections_accounts_refresh_create`

Refresh Account data (POST /v1/financial_connections/accounts/{account}/refresh). _(POST /api/stripe/financial/connections/accounts/refresh/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_financial_connections_accounts_subscribe_create`

Subscribe to data refreshes for an Account (POST /v1/financial_connections/accounts/{account}/subscribe). _(POST /api/stripe/financial/connections/accounts/subscribe/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_financial_connections_accounts_unsubscribe_create`

Unsubscribe from data refreshes for an Account (POST /v1/financial_connections/accounts/{account}/unsubscribe). _(POST /api/stripe/financial/connections/accounts/unsubscribe/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_financial_connections_sessions_create`

Create a Session (POST /v1/financial_connections/sessions). _(POST /api/stripe/financial/connections/sessions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_financial_connections_sessions_get`

Retrieve a Session (GET /v1/financial_connections/sessions/{session}). _(POST /api/stripe/financial/connections/sessions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_financial_connections_transactions_get`

Retrieve a Transaction (GET /v1/financial_connections/transactions/{transaction}). _(POST /api/stripe/financial/connections/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_financial_connections_transactions_list`

List Transactions (GET /v1/financial_connections/transactions). _(POST /api/stripe/financial/connections/transactions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_forwarding_requests_create`

Create a ForwardingRequest (POST /v1/forwarding/requests). _(POST /api/stripe/forwarding/requests/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_forwarding_requests_get`

Retrieve a ForwardingRequest (GET /v1/forwarding/requests/{id}). _(POST /api/stripe/forwarding/requests/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_forwarding_requests_list`

List all ForwardingRequests (GET /v1/forwarding/requests). _(POST /api/stripe/forwarding/requests/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_identity_verification_reports_get`

Retrieve a VerificationReport (GET /v1/identity/verification_reports/{report}). _(POST /api/stripe/identity/verification/reports/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `report` | string | Sim | Path param "report" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_identity_verification_reports_list`

List VerificationReports (GET /v1/identity/verification_reports). _(POST /api/stripe/identity/verification/reports/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_identity_verification_sessions_cancel_create`

Cancel a VerificationSession (POST /v1/identity/verification_sessions/{session}/cancel). _(POST /api/stripe/identity/verification/sessions/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_identity_verification_sessions_create`

Create a VerificationSession (POST /v1/identity/verification_sessions). _(POST /api/stripe/identity/verification/sessions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_identity_verification_sessions_get`

Retrieve a VerificationSession (GET /v1/identity/verification_sessions/{session}). _(POST /api/stripe/identity/verification/sessions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_identity_verification_sessions_list`

List VerificationSessions (GET /v1/identity/verification_sessions). _(POST /api/stripe/identity/verification/sessions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_identity_verification_sessions_redact_create`

Redact a VerificationSession (POST /v1/identity/verification_sessions/{session}/redact). _(POST /api/stripe/identity/verification/sessions/redact/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_identity_verification_sessions_update`

Update a VerificationSession (POST /v1/identity/verification_sessions/{session}). _(POST /api/stripe/identity/verification/sessions/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoice_payments_get`

Retrieve an InvoicePayment (GET /v1/invoice_payments/{invoice_payment}). _(POST /api/stripe/invoice/payments/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice_payment` | string | Sim | Path param "invoice_payment" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoice_payments_list`

List all payments for an invoice (GET /v1/invoice_payments). _(POST /api/stripe/invoice/payments/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoice_rendering_templates_archive_create`

Archive an invoice rendering template (POST /v1/invoice_rendering_templates/{template}/archive). _(POST /api/stripe/invoice/rendering/templates/archive/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `template` | string | Sim | Path param "template" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoice_rendering_templates_get`

Retrieve an invoice rendering template (GET /v1/invoice_rendering_templates/{template}). _(POST /api/stripe/invoice/rendering/templates/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `template` | string | Sim | Path param "template" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoice_rendering_templates_list`

List all invoice rendering templates (GET /v1/invoice_rendering_templates). _(POST /api/stripe/invoice/rendering/templates/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoice_rendering_templates_unarchive_create`

Unarchive an invoice rendering template (POST /v1/invoice_rendering_templates/{template}/unarchive). _(POST /api/stripe/invoice/rendering/templates/unarchive/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `template` | string | Sim | Path param "template" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoiceitems_create`

Create an invoice item (POST /v1/invoiceitems). _(POST /api/stripe/invoiceitems/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoiceitems_delete`

Delete an invoice item (DELETE /v1/invoiceitems/{invoiceitem}). _(POST /api/stripe/invoiceitems/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoiceitem` | string | Sim | Path param "invoiceitem" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoiceitems_get`

Retrieve an invoice item (GET /v1/invoiceitems/{invoiceitem}). _(POST /api/stripe/invoiceitems/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoiceitem` | string | Sim | Path param "invoiceitem" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoiceitems_list`

List all invoice items (GET /v1/invoiceitems). _(POST /api/stripe/invoiceitems/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoiceitems_update`

Update an invoice item (POST /v1/invoiceitems/{invoiceitem}). _(POST /api/stripe/invoiceitems/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoiceitem` | string | Sim | Path param "invoiceitem" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_add_lines_create`

Bulk add invoice line items (POST /v1/invoices/{invoice}/add_lines). _(POST /api/stripe/invoices/add/lines/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_attach_payment_create`

Attach a payment to an Invoice (POST /v1/invoices/{invoice}/attach_payment). _(POST /api/stripe/invoices/attach/payment/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_create`

Create an invoice (POST /v1/invoices). _(POST /api/stripe/invoices/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_create_preview_create`

Create a preview invoice (POST /v1/invoices/create_preview). _(POST /api/stripe/invoices/create/preview/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_delete`

Delete a draft invoice (DELETE /v1/invoices/{invoice}). _(POST /api/stripe/invoices/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_finalize_create`

Finalize an invoice (POST /v1/invoices/{invoice}/finalize). _(POST /api/stripe/invoices/finalize/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_get`

Retrieve an invoice (GET /v1/invoices/{invoice}). _(POST /api/stripe/invoices/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoices_lines_list`

Retrieve an invoice's line items (GET /v1/invoices/{invoice}/lines). _(POST /api/stripe/invoices/lines/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoices_lines_update`

Update an invoice's line item (POST /v1/invoices/{invoice}/lines/{line_item_id}). _(POST /api/stripe/invoices/lines/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `line_item_id` | string | Sim | Path param "line_item_id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `line_item_ids` | string[] | Não | Bulk mode: multiple values for line_item_id |

#### `stripe_invoices_list`

List all invoices (GET /v1/invoices). _(POST /api/stripe/invoices/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoices_mark_uncollectible_create`

Mark an invoice as uncollectible (POST /v1/invoices/{invoice}/mark_uncollectible). _(POST /api/stripe/invoices/mark/uncollectible/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_pay_create`

Pay an invoice (POST /v1/invoices/{invoice}/pay). _(POST /api/stripe/invoices/pay/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_remove_lines_create`

Bulk remove invoice line items (POST /v1/invoices/{invoice}/remove_lines). _(POST /api/stripe/invoices/remove/lines/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_search_list`

Search invoices (GET /v1/invoices/search). _(POST /api/stripe/invoices/search/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_invoices_send_create`

Send an invoice for manual payment (POST /v1/invoices/{invoice}/send). _(POST /api/stripe/invoices/send/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_update`

Update an invoice (POST /v1/invoices/{invoice}). _(POST /api/stripe/invoices/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_update_lines_create`

Bulk update invoice line items (POST /v1/invoices/{invoice}/update_lines). _(POST /api/stripe/invoices/update/lines/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_invoices_void_create`

Void an invoice (POST /v1/invoices/{invoice}/void). _(POST /api/stripe/invoices/void/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `invoice` | string | Sim | Path param "invoice" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_authorizations_approve_create`

Approve an authorization (POST /v1/issuing/authorizations/{authorization}/approve). _(POST /api/stripe/issuing/authorizations/approve/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_authorizations_decline_create`

Decline an authorization (POST /v1/issuing/authorizations/{authorization}/decline). _(POST /api/stripe/issuing/authorizations/decline/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_authorizations_get`

Retrieve an authorization (GET /v1/issuing/authorizations/{authorization}). _(POST /api/stripe/issuing/authorizations/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_authorizations_list`

List all authorizations (GET /v1/issuing/authorizations). _(POST /api/stripe/issuing/authorizations/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_authorizations_update`

Update an authorization (POST /v1/issuing/authorizations/{authorization}). _(POST /api/stripe/issuing/authorizations/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_cardholders_create`

Create a cardholder (POST /v1/issuing/cardholders). _(POST /api/stripe/issuing/cardholders/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_cardholders_get`

Retrieve a cardholder (GET /v1/issuing/cardholders/{cardholder}). _(POST /api/stripe/issuing/cardholders/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `cardholder` | string | Sim | Path param "cardholder" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_cardholders_list`

List all cardholders (GET /v1/issuing/cardholders). _(POST /api/stripe/issuing/cardholders/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_cardholders_update`

Update a cardholder (POST /v1/issuing/cardholders/{cardholder}). _(POST /api/stripe/issuing/cardholders/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `cardholder` | string | Sim | Path param "cardholder" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_cards_create`

Create a card (POST /v1/issuing/cards). _(POST /api/stripe/issuing/cards/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_cards_get`

Retrieve a card (GET /v1/issuing/cards/{card}). _(POST /api/stripe/issuing/cards/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `card` | string | Sim | Path param "card" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_cards_list`

List all cards (GET /v1/issuing/cards). _(POST /api/stripe/issuing/cards/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_cards_update`

Update a card (POST /v1/issuing/cards/{card}). _(POST /api/stripe/issuing/cards/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `card` | string | Sim | Path param "card" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_disputes_create`

Create a dispute (POST /v1/issuing/disputes). _(POST /api/stripe/issuing/disputes/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_disputes_get`

Retrieve a dispute (GET /v1/issuing/disputes/{dispute}). _(POST /api/stripe/issuing/disputes/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `dispute` | string | Sim | Path param "dispute" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_disputes_list`

List all disputes (GET /v1/issuing/disputes). _(POST /api/stripe/issuing/disputes/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_disputes_submit_create`

Submit a dispute (POST /v1/issuing/disputes/{dispute}/submit). _(POST /api/stripe/issuing/disputes/submit/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `dispute` | string | Sim | Path param "dispute" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_disputes_update`

Update a dispute (POST /v1/issuing/disputes/{dispute}). _(POST /api/stripe/issuing/disputes/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `dispute` | string | Sim | Path param "dispute" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_personalization_designs_create`

Create a personalization design (POST /v1/issuing/personalization_designs). _(POST /api/stripe/issuing/personalization/designs/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_personalization_designs_get`

Retrieve a personalization design (GET /v1/issuing/personalization_designs/{personalization_design}). _(POST /api/stripe/issuing/personalization/designs/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `personalization_design` | string | Sim | Path param "personalization_design" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_personalization_designs_list`

List all personalization designs (GET /v1/issuing/personalization_designs). _(POST /api/stripe/issuing/personalization/designs/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_personalization_designs_update`

Update a personalization design (POST /v1/issuing/personalization_designs/{personalization_design}). _(POST /api/stripe/issuing/personalization/designs/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `personalization_design` | string | Sim | Path param "personalization_design" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_physical_bundles_get`

Retrieve a physical bundle (GET /v1/issuing/physical_bundles/{physical_bundle}). _(POST /api/stripe/issuing/physical/bundles/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `physical_bundle` | string | Sim | Path param "physical_bundle" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_physical_bundles_list`

List all physical bundles (GET /v1/issuing/physical_bundles). _(POST /api/stripe/issuing/physical/bundles/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_settlements_get`

Retrieve a settlement (GET /v1/issuing/settlements/{settlement}). _(POST /api/stripe/issuing/settlements/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `settlement` | string | Sim | Path param "settlement" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_settlements_update`

Update a settlement (POST /v1/issuing/settlements/{settlement}). _(POST /api/stripe/issuing/settlements/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `settlement` | string | Sim | Path param "settlement" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_tokens_get`

Retrieve an issuing token (GET /v1/issuing/tokens/{token}). _(POST /api/stripe/issuing/tokens/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `token` | string | Sim | Path param "token" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_tokens_list`

List all issuing tokens for card (GET /v1/issuing/tokens). _(POST /api/stripe/issuing/tokens/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_tokens_update`

Update a token status (POST /v1/issuing/tokens/{token}). _(POST /api/stripe/issuing/tokens/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `token` | string | Sim | Path param "token" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_issuing_transactions_get`

Retrieve a transaction (GET /v1/issuing/transactions/{transaction}). _(POST /api/stripe/issuing/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_transactions_list`

List all transactions (GET /v1/issuing/transactions). _(POST /api/stripe/issuing/transactions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_issuing_transactions_update`

Update a transaction (POST /v1/issuing/transactions/{transaction}). _(POST /api/stripe/issuing/transactions/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_link_account_sessions_create`

Create a Session (POST /v1/link_account_sessions). _(POST /api/stripe/link/account/sessions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_link_account_sessions_get`

Retrieve a Session (GET /v1/link_account_sessions/{session}). _(POST /api/stripe/link/account/sessions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `session` | string | Sim | Path param "session" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_linked_accounts_disconnect_create`

Disconnect an Account (POST /v1/linked_accounts/{account}/disconnect). _(POST /api/stripe/linked/accounts/disconnect/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_linked_accounts_get`

Retrieve an Account (GET /v1/linked_accounts/{account}). _(POST /api/stripe/linked/accounts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_linked_accounts_list`

List Accounts (GET /v1/linked_accounts). _(POST /api/stripe/linked/accounts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_linked_accounts_owners_list`

List Account Owners (GET /v1/linked_accounts/{account}/owners). _(POST /api/stripe/linked/accounts/owners/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_linked_accounts_refresh_create`

Refresh Account data (POST /v1/linked_accounts/{account}/refresh). _(POST /api/stripe/linked/accounts/refresh/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Sim | Path param "account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_list_accounts`

Lista as conexões (contas) Stripe vinculadas a este install — id, label. _(POST /api/stripe/list/accounts)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |

#### `stripe_mandates_get`

Retrieve a Mandate (GET /v1/mandates/{mandate}). _(POST /api/stripe/mandates/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `mandate` | string | Sim | Path param "mandate" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_attempt_records_get`

Retrieve a Payment Attempt Record (GET /v1/payment_attempt_records/{id}). _(POST /api/stripe/payment/attempt/records/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_payment_attempt_records_list`

List Payment Attempt Records (GET /v1/payment_attempt_records). _(POST /api/stripe/payment/attempt/records/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_intents_amount_details_line_items_list`

List all PaymentIntent LineItems (GET /v1/payment_intents/{intent}/amount_details_line_items). _(POST /api/stripe/payment/intents/amount/details/line/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_intents_apply_customer_balance_create`

Reconcile a customer_balance PaymentIntent (POST /v1/payment_intents/{intent}/apply_customer_balance). _(POST /api/stripe/payment/intents/apply/customer/balance/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_intents_cancel_create`

Cancel a PaymentIntent (POST /v1/payment_intents/{intent}/cancel). _(POST /api/stripe/payment/intents/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_intents_capture_create`

Capture a PaymentIntent (POST /v1/payment_intents/{intent}/capture). _(POST /api/stripe/payment/intents/capture/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_intents_confirm_create`

Confirm a PaymentIntent (POST /v1/payment_intents/{intent}/confirm). _(POST /api/stripe/payment/intents/confirm/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_intents_create`

Create a PaymentIntent (POST /v1/payment_intents). _(POST /api/stripe/payment/intents/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_intents_get`

Retrieve a PaymentIntent (GET /v1/payment_intents/{intent}). _(POST /api/stripe/payment/intents/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_intents_increment_authorization_create`

Increment an authorization (POST /v1/payment_intents/{intent}/increment_authorization). _(POST /api/stripe/payment/intents/increment/authorization/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_intents_list`

List all PaymentIntents (GET /v1/payment_intents). _(POST /api/stripe/payment/intents/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_intents_search_list`

Search PaymentIntents (GET /v1/payment_intents/search). _(POST /api/stripe/payment/intents/search/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_intents_update`

Update a PaymentIntent (POST /v1/payment_intents/{intent}). _(POST /api/stripe/payment/intents/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_intents_verify_microdeposits_create`

Verify microdeposits on a PaymentIntent (POST /v1/payment_intents/{intent}/verify_microdeposits). _(POST /api/stripe/payment/intents/verify/microdeposits/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_links_create`

Create a payment link (POST /v1/payment_links). _(POST /api/stripe/payment/links/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_links_get`

Retrieve payment link (GET /v1/payment_links/{payment_link}). _(POST /api/stripe/payment/links/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_link` | string | Sim | Path param "payment_link" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_links_line_items_list`

Retrieve a payment link's line items (GET /v1/payment_links/{payment_link}/line_items). _(POST /api/stripe/payment/links/line/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_link` | string | Sim | Path param "payment_link" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_links_list`

List all payment links (GET /v1/payment_links). _(POST /api/stripe/payment/links/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_links_update`

Update a payment link (POST /v1/payment_links/{payment_link}). _(POST /api/stripe/payment/links/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_link` | string | Sim | Path param "payment_link" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_method_configurations_create`

Create a payment method configuration (POST /v1/payment_method_configurations). _(POST /api/stripe/payment/method/configurations/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_method_configurations_get`

Retrieve payment method configuration (GET /v1/payment_method_configurations/{configuration}). _(POST /api/stripe/payment/method/configurations/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `configuration` | string | Sim | Path param "configuration" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_method_configurations_list`

List payment method configurations (GET /v1/payment_method_configurations). _(POST /api/stripe/payment/method/configurations/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_method_configurations_update`

Update payment method configuration (POST /v1/payment_method_configurations/{configuration}). _(POST /api/stripe/payment/method/configurations/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `configuration` | string | Sim | Path param "configuration" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_method_domains_create`

Create a payment method domain (POST /v1/payment_method_domains). _(POST /api/stripe/payment/method/domains/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_method_domains_get`

Retrieve a payment method domain (GET /v1/payment_method_domains/{payment_method_domain}). _(POST /api/stripe/payment/method/domains/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_method_domain` | string | Sim | Path param "payment_method_domain" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_method_domains_list`

List payment method domains (GET /v1/payment_method_domains). _(POST /api/stripe/payment/method/domains/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_method_domains_update`

Update a payment method domain (POST /v1/payment_method_domains/{payment_method_domain}). _(POST /api/stripe/payment/method/domains/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_method_domain` | string | Sim | Path param "payment_method_domain" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_method_domains_validate_create`

Validate an existing payment method domain (POST /v1/payment_method_domains/{payment_method_domain}/validate). _(POST /api/stripe/payment/method/domains/validate/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_method_domain` | string | Sim | Path param "payment_method_domain" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_methods_attach_create`

Attach a PaymentMethod to a Customer (POST /v1/payment_methods/{payment_method}/attach). _(POST /api/stripe/payment/methods/attach/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_method` | string | Sim | Path param "payment_method" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_methods_create`

Create a PaymentMethod (POST /v1/payment_methods). _(POST /api/stripe/payment/methods/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_methods_detach_create`

Detach a PaymentMethod from a Customer (POST /v1/payment_methods/{payment_method}/detach). _(POST /api/stripe/payment/methods/detach/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_method` | string | Sim | Path param "payment_method" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_methods_get`

Retrieve a PaymentMethod (GET /v1/payment_methods/{payment_method}). _(POST /api/stripe/payment/methods/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_method` | string | Sim | Path param "payment_method" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_methods_list`

List PaymentMethods (GET /v1/payment_methods). _(POST /api/stripe/payment/methods/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_methods_update`

Update a PaymentMethod (POST /v1/payment_methods/{payment_method}). _(POST /api/stripe/payment/methods/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payment_method` | string | Sim | Path param "payment_method" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_records_get`

Retrieve a Payment Record (GET /v1/payment_records/{id}). _(POST /api/stripe/payment/records/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_payment_records_list`

List Payment Records (GET /v1/payment_records). _(POST /api/stripe/payment/records/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payment_records_report_payment_attempt_canceled_create`

Report payment attempt canceled (POST /v1/payment_records/{id}/report_payment_attempt_canceled). _(POST /api/stripe/payment/records/report/payment/attempt/canceled/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_payment_records_report_payment_attempt_create`

Report a payment attempt (POST /v1/payment_records/{id}/report_payment_attempt). _(POST /api/stripe/payment/records/report/payment/attempt/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_payment_records_report_payment_attempt_failed_create`

Report payment attempt failed (POST /v1/payment_records/{id}/report_payment_attempt_failed). _(POST /api/stripe/payment/records/report/payment/attempt/failed/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_payment_records_report_payment_attempt_guaranteed_create`

Report payment attempt guaranteed (POST /v1/payment_records/{id}/report_payment_attempt_guaranteed). _(POST /api/stripe/payment/records/report/payment/attempt/guaranteed/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_payment_records_report_payment_attempt_informational_create`

Report payment attempt informational (POST /v1/payment_records/{id}/report_payment_attempt_informational). _(POST /api/stripe/payment/records/report/payment/attempt/informational/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_payment_records_report_payment_create`

Report a payment (POST /v1/payment_records/report_payment). _(POST /api/stripe/payment/records/report/payment/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payment_records_report_refund_create`

Report a refund (POST /v1/payment_records/{id}/report_refund). _(POST /api/stripe/payment/records/report/refund/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_payouts_cancel_create`

Cancel a payout (POST /v1/payouts/{payout}/cancel). _(POST /api/stripe/payouts/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payout` | string | Sim | Path param "payout" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payouts_create`

Create a payout (POST /v1/payouts). _(POST /api/stripe/payouts/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payouts_get`

Retrieve a payout (GET /v1/payouts/{payout}). _(POST /api/stripe/payouts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payout` | string | Sim | Path param "payout" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payouts_list`

List all payouts (GET /v1/payouts). _(POST /api/stripe/payouts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_payouts_reverse_create`

Reverse a payout (POST /v1/payouts/{payout}/reverse). _(POST /api/stripe/payouts/reverse/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payout` | string | Sim | Path param "payout" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_payouts_update`

Update a payout (POST /v1/payouts/{payout}). _(POST /api/stripe/payouts/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `payout` | string | Sim | Path param "payout" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_plans_create`

Create a plan (POST /v1/plans). _(POST /api/stripe/plans/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_plans_delete`

Delete a plan (DELETE /v1/plans/{plan}). _(POST /api/stripe/plans/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `plan` | string | Sim | Path param "plan" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_plans_get`

Retrieve a plan (GET /v1/plans/{plan}). _(POST /api/stripe/plans/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `plan` | string | Sim | Path param "plan" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_plans_list`

List all plans (GET /v1/plans). _(POST /api/stripe/plans/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_plans_update`

Update a plan (POST /v1/plans/{plan}). _(POST /api/stripe/plans/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `plan` | string | Sim | Path param "plan" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_prices_create`

Create a price (POST /v1/prices). _(POST /api/stripe/prices/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_prices_get`

Retrieve a price (GET /v1/prices/{price}). _(POST /api/stripe/prices/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `price` | string | Sim | Path param "price" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_prices_list`

List all prices (GET /v1/prices). _(POST /api/stripe/prices/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_prices_search_list`

Search prices (GET /v1/prices/search). _(POST /api/stripe/prices/search/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_prices_update`

Update a price (POST /v1/prices/{price}). _(POST /api/stripe/prices/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `price` | string | Sim | Path param "price" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_products_create`

Create a product (POST /v1/products). _(POST /api/stripe/products/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_products_delete`

Delete a product (DELETE /v1/products/{id}). _(POST /api/stripe/products/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_products_features_create`

Attach a feature to a product (POST /v1/products/{product}/features). _(POST /api/stripe/products/features/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `product` | string | Sim | Path param "product" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_products_features_delete`

Remove a feature from a product (DELETE /v1/products/{product}/features/{id}). _(POST /api/stripe/products/features/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `product` | string | Sim | Path param "product" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_products_features_get`

Retrieve a product_feature (GET /v1/products/{product}/features/{id}). _(POST /api/stripe/products/features/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `product` | string | Sim | Path param "product" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_products_features_list`

List all features attached to a product (GET /v1/products/{product}/features). _(POST /api/stripe/products/features/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `product` | string | Sim | Path param "product" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_products_get`

Retrieve a product (GET /v1/products/{id}). _(POST /api/stripe/products/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_products_list`

List all products (GET /v1/products). _(POST /api/stripe/products/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_products_search_list`

Search products (GET /v1/products/search). _(POST /api/stripe/products/search/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_products_update`

Update a product (POST /v1/products/{id}). _(POST /api/stripe/products/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_promotion_codes_create`

Create a promotion code (POST /v1/promotion_codes). _(POST /api/stripe/promotion/codes/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_promotion_codes_get`

Retrieve a promotion code (GET /v1/promotion_codes/{promotion_code}). _(POST /api/stripe/promotion/codes/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `promotion_code` | string | Sim | Path param "promotion_code" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_promotion_codes_list`

List all promotion codes (GET /v1/promotion_codes). _(POST /api/stripe/promotion/codes/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_promotion_codes_update`

Update a promotion code (POST /v1/promotion_codes/{promotion_code}). _(POST /api/stripe/promotion/codes/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `promotion_code` | string | Sim | Path param "promotion_code" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_quotes_accept_create`

Accept a quote (POST /v1/quotes/{quote}/accept). _(POST /api/stripe/quotes/accept/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `quote` | string | Sim | Path param "quote" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_quotes_cancel_create`

Cancel a quote (POST /v1/quotes/{quote}/cancel). _(POST /api/stripe/quotes/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `quote` | string | Sim | Path param "quote" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_quotes_computed_upfront_line_items_list`

Retrieve a quote's upfront line items (GET /v1/quotes/{quote}/computed_upfront_line_items). _(POST /api/stripe/quotes/computed/upfront/line/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `quote` | string | Sim | Path param "quote" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_quotes_create`

Create a quote (POST /v1/quotes). _(POST /api/stripe/quotes/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_quotes_finalize_create`

Finalize a quote (POST /v1/quotes/{quote}/finalize). _(POST /api/stripe/quotes/finalize/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `quote` | string | Sim | Path param "quote" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_quotes_get`

Retrieve a quote (GET /v1/quotes/{quote}). _(POST /api/stripe/quotes/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `quote` | string | Sim | Path param "quote" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_quotes_line_items_list`

Retrieve a quote's line items (GET /v1/quotes/{quote}/line_items). _(POST /api/stripe/quotes/line/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `quote` | string | Sim | Path param "quote" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_quotes_list`

List all quotes (GET /v1/quotes). _(POST /api/stripe/quotes/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_quotes_pdf_list`

Download quote PDF (GET /v1/quotes/{quote}/pdf). _(POST /api/stripe/quotes/pdf/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `quote` | string | Sim | Path param "quote" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_quotes_update`

Update a quote (POST /v1/quotes/{quote}). _(POST /api/stripe/quotes/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `quote` | string | Sim | Path param "quote" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_radar_early_fraud_warnings_get`

Retrieve an early fraud warning (GET /v1/radar/early_fraud_warnings/{early_fraud_warning}). _(POST /api/stripe/radar/early/fraud/warnings/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `early_fraud_warning` | string | Sim | Path param "early_fraud_warning" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_radar_early_fraud_warnings_list`

List all early fraud warnings (GET /v1/radar/early_fraud_warnings). _(POST /api/stripe/radar/early/fraud/warnings/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_radar_payment_evaluations_create`

Create a Payment Evaluation (POST /v1/radar/payment_evaluations). _(POST /api/stripe/radar/payment/evaluations/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_radar_value_list_items_create`

Create a value list item (POST /v1/radar/value_list_items). _(POST /api/stripe/radar/value/list/items/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_radar_value_list_items_delete`

Delete a value list item (DELETE /v1/radar/value_list_items/{item}). _(POST /api/stripe/radar/value/list/items/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `item` | string | Sim | Path param "item" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_radar_value_list_items_get`

Retrieve a value list item (GET /v1/radar/value_list_items/{item}). _(POST /api/stripe/radar/value/list/items/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `item` | string | Sim | Path param "item" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_radar_value_list_items_list`

List all value list items (GET /v1/radar/value_list_items). _(POST /api/stripe/radar/value/list/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_radar_value_lists_create`

Create a value list (POST /v1/radar/value_lists). _(POST /api/stripe/radar/value/lists/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_radar_value_lists_delete`

Delete a value list (DELETE /v1/radar/value_lists/{value_list}). _(POST /api/stripe/radar/value/lists/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `value_list` | string | Sim | Path param "value_list" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_radar_value_lists_get`

Retrieve a value list (GET /v1/radar/value_lists/{value_list}). _(POST /api/stripe/radar/value/lists/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `value_list` | string | Sim | Path param "value_list" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_radar_value_lists_list`

List all value lists (GET /v1/radar/value_lists). _(POST /api/stripe/radar/value/lists/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_radar_value_lists_update`

Update a value list (POST /v1/radar/value_lists/{value_list}). _(POST /api/stripe/radar/value/lists/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `value_list` | string | Sim | Path param "value_list" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_refunds_cancel_create`

Cancel a refund (POST /v1/refunds/{refund}/cancel). _(POST /api/stripe/refunds/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `refund` | string | Sim | Path param "refund" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_refunds_create`

Create a refund (POST /v1/refunds). _(POST /api/stripe/refunds/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_refunds_get`

Retrieve a refund (GET /v1/refunds/{refund}). _(POST /api/stripe/refunds/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `refund` | string | Sim | Path param "refund" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_refunds_list`

List all refunds (GET /v1/refunds). _(POST /api/stripe/refunds/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_refunds_update`

Update a refund (POST /v1/refunds/{refund}). _(POST /api/stripe/refunds/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `refund` | string | Sim | Path param "refund" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_reporting_report_runs_create`

Create a Report Run (POST /v1/reporting/report_runs). _(POST /api/stripe/reporting/report/runs/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_reporting_report_runs_get`

Retrieve a Report Run (GET /v1/reporting/report_runs/{report_run}). _(POST /api/stripe/reporting/report/runs/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `report_run` | string | Sim | Path param "report_run" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_reporting_report_runs_list`

List all Report Runs (GET /v1/reporting/report_runs). _(POST /api/stripe/reporting/report/runs/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_reporting_report_types_get`

Retrieve a Report Type (GET /v1/reporting/report_types/{report_type}). _(POST /api/stripe/reporting/report/types/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `report_type` | string | Sim | Path param "report_type" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_reporting_report_types_list`

List all Report Types (GET /v1/reporting/report_types). _(POST /api/stripe/reporting/report/types/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_reviews_approve_create`

Approve a review (POST /v1/reviews/{review}/approve). _(POST /api/stripe/reviews/approve/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `review` | string | Sim | Path param "review" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_reviews_get`

Retrieve a review (GET /v1/reviews/{review}). _(POST /api/stripe/reviews/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `review` | string | Sim | Path param "review" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_reviews_list`

List all open reviews (GET /v1/reviews). _(POST /api/stripe/reviews/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_setup_attempts_list`

List all SetupAttempts (GET /v1/setup_attempts). _(POST /api/stripe/setup/attempts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_setup_intents_cancel_create`

Cancel a SetupIntent (POST /v1/setup_intents/{intent}/cancel). _(POST /api/stripe/setup/intents/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_setup_intents_confirm_create`

Confirm a SetupIntent (POST /v1/setup_intents/{intent}/confirm). _(POST /api/stripe/setup/intents/confirm/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_setup_intents_create`

Create a SetupIntent (POST /v1/setup_intents). _(POST /api/stripe/setup/intents/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_setup_intents_get`

Retrieve a SetupIntent (GET /v1/setup_intents/{intent}). _(POST /api/stripe/setup/intents/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_setup_intents_list`

List all SetupIntents (GET /v1/setup_intents). _(POST /api/stripe/setup/intents/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_setup_intents_update`

Update a SetupIntent (POST /v1/setup_intents/{intent}). _(POST /api/stripe/setup/intents/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_setup_intents_verify_microdeposits_create`

Verify microdeposits on a SetupIntent (POST /v1/setup_intents/{intent}/verify_microdeposits). _(POST /api/stripe/setup/intents/verify/microdeposits/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `intent` | string | Sim | Path param "intent" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_shipping_rates_create`

Create a shipping rate (POST /v1/shipping_rates). _(POST /api/stripe/shipping/rates/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_shipping_rates_get`

Retrieve a shipping rate (GET /v1/shipping_rates/{shipping_rate_token}). _(POST /api/stripe/shipping/rates/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `shipping_rate_token` | string | Sim | Path param "shipping_rate_token" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_shipping_rates_list`

List all shipping rates (GET /v1/shipping_rates). _(POST /api/stripe/shipping/rates/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_shipping_rates_update`

Update a shipping rate (POST /v1/shipping_rates/{shipping_rate_token}). _(POST /api/stripe/shipping/rates/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `shipping_rate_token` | string | Sim | Path param "shipping_rate_token" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_sigma_saved_queries_update`

Update an existing Sigma Query (POST /v1/sigma/saved_queries/{id}). _(POST /api/stripe/sigma/saved/queries/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_sigma_scheduled_query_runs_get`

Retrieve a scheduled query run (GET /v1/sigma/scheduled_query_runs/{scheduled_query_run}). _(POST /api/stripe/sigma/scheduled/query/runs/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `scheduled_query_run` | string | Sim | Path param "scheduled_query_run" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_sigma_scheduled_query_runs_list`

List all scheduled query runs (GET /v1/sigma/scheduled_query_runs). _(POST /api/stripe/sigma/scheduled/query/runs/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_sources_create`

Create a source (POST /v1/sources). _(POST /api/stripe/sources/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_sources_get`

Retrieve a source (GET /v1/sources/{source}). _(POST /api/stripe/sources/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `source` | string | Sim | Path param "source" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_sources_mandate_notifications_get`

Retrieve a Source MandateNotification (GET /v1/sources/{source}/mandate_notifications/{mandate_notification}). _(POST /api/stripe/sources/mandate/notifications/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `source` | string | Sim | Path param "source" (obrigatório) |
| `mandate_notification` | string | Sim | Path param "mandate_notification" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_sources_source_transactions_get`

Retrieve a source transaction (GET /v1/sources/{source}/source_transactions/{source_transaction}). _(POST /api/stripe/sources/source/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `source` | string | Sim | Path param "source" (obrigatório) |
| `source_transaction` | string | Sim | Path param "source_transaction" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_sources_source_transactions_list`

GetSourcesSourceSourceTransactions (GET /v1/sources/{source}/source_transactions). _(POST /api/stripe/sources/source/transactions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `source` | string | Sim | Path param "source" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_sources_update`

Update a source (POST /v1/sources/{source}). _(POST /api/stripe/sources/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `source` | string | Sim | Path param "source" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_sources_verify_create`

PostSourcesSourceVerify (POST /v1/sources/{source}/verify). _(POST /api/stripe/sources/verify/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `source` | string | Sim | Path param "source" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscription_items_create`

Create a subscription item (POST /v1/subscription_items). _(POST /api/stripe/subscription/items/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscription_items_delete`

Delete a subscription item (DELETE /v1/subscription_items/{item}). _(POST /api/stripe/subscription/items/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `item` | string | Sim | Path param "item" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscription_items_get`

Retrieve a subscription item (GET /v1/subscription_items/{item}). _(POST /api/stripe/subscription/items/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `item` | string | Sim | Path param "item" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_subscription_items_list`

List all subscription items (GET /v1/subscription_items). _(POST /api/stripe/subscription/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_subscription_items_update`

Update a subscription item (POST /v1/subscription_items/{item}). _(POST /api/stripe/subscription/items/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `item` | string | Sim | Path param "item" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscription_schedules_cancel_create`

Cancel a schedule (POST /v1/subscription_schedules/{schedule}/cancel). _(POST /api/stripe/subscription/schedules/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `schedule` | string | Sim | Path param "schedule" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscription_schedules_create`

Create a schedule (POST /v1/subscription_schedules). _(POST /api/stripe/subscription/schedules/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscription_schedules_get`

Retrieve a schedule (GET /v1/subscription_schedules/{schedule}). _(POST /api/stripe/subscription/schedules/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `schedule` | string | Sim | Path param "schedule" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_subscription_schedules_list`

List all schedules (GET /v1/subscription_schedules). _(POST /api/stripe/subscription/schedules/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_subscription_schedules_release_create`

Release a schedule (POST /v1/subscription_schedules/{schedule}/release). _(POST /api/stripe/subscription/schedules/release/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `schedule` | string | Sim | Path param "schedule" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscription_schedules_update`

Update a schedule (POST /v1/subscription_schedules/{schedule}). _(POST /api/stripe/subscription/schedules/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `schedule` | string | Sim | Path param "schedule" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscriptions_create`

Create a subscription (POST /v1/subscriptions). _(POST /api/stripe/subscriptions/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscriptions_delete`

Cancel a subscription (DELETE /v1/subscriptions/{subscription_exposed_id}). _(POST /api/stripe/subscriptions/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_subscriptions_discount_delete`

Delete a subscription discount (DELETE /v1/subscriptions/{subscription_exposed_id}/discount). _(POST /api/stripe/subscriptions/discount/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_subscriptions_get`

Retrieve a subscription (GET /v1/subscriptions/{subscription_exposed_id}). _(POST /api/stripe/subscriptions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_subscriptions_list`

List subscriptions (GET /v1/subscriptions). _(POST /api/stripe/subscriptions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_subscriptions_migrate_create`

Migrate a subscription (POST /v1/subscriptions/{subscription}/migrate). _(POST /api/stripe/subscriptions/migrate/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `subscription` | string | Sim | Path param "subscription" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscriptions_resume_create`

Resume a subscription (POST /v1/subscriptions/{subscription}/resume). _(POST /api/stripe/subscriptions/resume/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `subscription` | string | Sim | Path param "subscription" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_subscriptions_search_list`

Search subscriptions (GET /v1/subscriptions/search). _(POST /api/stripe/subscriptions/search/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_subscriptions_update`

Update a subscription (POST /v1/subscriptions/{subscription_exposed_id}). _(POST /api/stripe/subscriptions/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `subscription_exposed_id` | string | Sim | Path param "subscription_exposed_id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `subscription_exposed_ids` | string[] | Não | Bulk mode: multiple values for subscription_exposed_id |

#### `stripe_tax_associations_find_list`

Find a Tax Association (GET /v1/tax/associations/find). _(POST /api/stripe/tax/associations/find/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_calculations_create`

Create a Calculation (POST /v1/tax/calculations). _(POST /api/stripe/tax/calculations/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tax_calculations_get`

Retrieve a Calculation (GET /v1/tax/calculations/{calculation}). _(POST /api/stripe/tax/calculations/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `calculation` | string | Sim | Path param "calculation" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_calculations_line_items_list`

Retrieve a Calculation's line items (GET /v1/tax/calculations/{calculation}/line_items). _(POST /api/stripe/tax/calculations/line/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `calculation` | string | Sim | Path param "calculation" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_codes_get`

Retrieve a tax code (GET /v1/tax_codes/{id}). _(POST /api/stripe/tax/codes/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_tax_codes_list`

List all tax codes (GET /v1/tax_codes). _(POST /api/stripe/tax/codes/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_ids_create`

Create a tax ID (POST /v1/tax_ids). _(POST /api/stripe/tax/ids/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tax_ids_delete`

Delete a tax ID (DELETE /v1/tax_ids/{id}). _(POST /api/stripe/tax/ids/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_tax_ids_get`

Retrieve a tax ID (GET /v1/tax_ids/{id}). _(POST /api/stripe/tax/ids/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_tax_ids_list`

List all tax IDs (GET /v1/tax_ids). _(POST /api/stripe/tax/ids/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_rates_create`

Create a tax rate (POST /v1/tax_rates). _(POST /api/stripe/tax/rates/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tax_rates_get`

Retrieve a tax rate (GET /v1/tax_rates/{tax_rate}). _(POST /api/stripe/tax/rates/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `tax_rate` | string | Sim | Path param "tax_rate" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_rates_list`

List all tax rates (GET /v1/tax_rates). _(POST /api/stripe/tax/rates/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_rates_update`

Update a tax rate (POST /v1/tax_rates/{tax_rate}). _(POST /api/stripe/tax/rates/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `tax_rate` | string | Sim | Path param "tax_rate" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tax_registrations_create`

Create a registration (POST /v1/tax/registrations). _(POST /api/stripe/tax/registrations/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tax_registrations_get`

Retrieve a registration (GET /v1/tax/registrations/{id}). _(POST /api/stripe/tax/registrations/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_tax_registrations_list`

List registrations (GET /v1/tax/registrations). _(POST /api/stripe/tax/registrations/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_registrations_update`

Update a registration (POST /v1/tax/registrations/{id}). _(POST /api/stripe/tax/registrations/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_tax_settings_create`

Update settings (POST /v1/tax/settings). _(POST /api/stripe/tax/settings/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tax_settings_list`

Retrieve settings (GET /v1/tax/settings). _(POST /api/stripe/tax/settings/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_transactions_create_from_calculation_create`

Create a Transaction from a Calculation (POST /v1/tax/transactions/create_from_calculation). _(POST /api/stripe/tax/transactions/create/from/calculation/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tax_transactions_create_reversal_create`

Create a reversal Transaction (POST /v1/tax/transactions/create_reversal). _(POST /api/stripe/tax/transactions/create/reversal/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tax_transactions_get`

Retrieve a Transaction (GET /v1/tax/transactions/{transaction}). _(POST /api/stripe/tax/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_tax_transactions_line_items_list`

Retrieve a Transaction's line items (GET /v1/tax/transactions/{transaction}/line_items). _(POST /api/stripe/tax/transactions/line/items/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_terminal_configurations_create`

Create a Configuration (POST /v1/terminal/configurations). _(POST /api/stripe/terminal/configurations/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_configurations_delete`

Delete a Configuration (DELETE /v1/terminal/configurations/{configuration}). _(POST /api/stripe/terminal/configurations/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `configuration` | string | Sim | Path param "configuration" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_configurations_get`

Retrieve a Configuration (GET /v1/terminal/configurations/{configuration}). _(POST /api/stripe/terminal/configurations/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `configuration` | string | Sim | Path param "configuration" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_terminal_configurations_list`

List all Configurations (GET /v1/terminal/configurations). _(POST /api/stripe/terminal/configurations/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_terminal_configurations_update`

Update a Configuration (POST /v1/terminal/configurations/{configuration}). _(POST /api/stripe/terminal/configurations/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `configuration` | string | Sim | Path param "configuration" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_connection_tokens_create`

Create a Connection Token (POST /v1/terminal/connection_tokens). _(POST /api/stripe/terminal/connection/tokens/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_locations_create`

Create a Location (POST /v1/terminal/locations). _(POST /api/stripe/terminal/locations/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_locations_delete`

Delete a Location (DELETE /v1/terminal/locations/{location}). _(POST /api/stripe/terminal/locations/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `location` | string | Sim | Path param "location" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_locations_get`

Retrieve a Location (GET /v1/terminal/locations/{location}). _(POST /api/stripe/terminal/locations/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `location` | string | Sim | Path param "location" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_terminal_locations_list`

List all Locations (GET /v1/terminal/locations). _(POST /api/stripe/terminal/locations/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_terminal_locations_update`

Update a Location (POST /v1/terminal/locations/{location}). _(POST /api/stripe/terminal/locations/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `location` | string | Sim | Path param "location" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_onboarding_links_create`

Create an Onboarding Link (POST /v1/terminal/onboarding_links). _(POST /api/stripe/terminal/onboarding/links/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_cancel_action_create`

Cancel the current reader action (POST /v1/terminal/readers/{reader}/cancel_action). _(POST /api/stripe/terminal/readers/cancel/action/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_collect_inputs_create`

Collect inputs using a Reader (POST /v1/terminal/readers/{reader}/collect_inputs). _(POST /api/stripe/terminal/readers/collect/inputs/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_collect_payment_method_create`

Hand off a PaymentIntent to a Reader and collect card details (POST /v1/terminal/readers/{reader}/collect_payment_method). _(POST /api/stripe/terminal/readers/collect/payment/method/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_confirm_payment_intent_create`

Confirm a PaymentIntent on the Reader (POST /v1/terminal/readers/{reader}/confirm_payment_intent). _(POST /api/stripe/terminal/readers/confirm/payment/intent/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_create`

Create a Reader (POST /v1/terminal/readers). _(POST /api/stripe/terminal/readers/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_delete`

Delete a Reader (DELETE /v1/terminal/readers/{reader}). _(POST /api/stripe/terminal/readers/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_get`

Retrieve a Reader (GET /v1/terminal/readers/{reader}). _(POST /api/stripe/terminal/readers/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_terminal_readers_list`

List all Readers (GET /v1/terminal/readers). _(POST /api/stripe/terminal/readers/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_terminal_readers_process_payment_intent_create`

Hand-off a PaymentIntent to a Reader (POST /v1/terminal/readers/{reader}/process_payment_intent). _(POST /api/stripe/terminal/readers/process/payment/intent/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_process_setup_intent_create`

Hand-off a SetupIntent to a Reader (POST /v1/terminal/readers/{reader}/process_setup_intent). _(POST /api/stripe/terminal/readers/process/setup/intent/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_refund_payment_create`

Refund a Charge or a PaymentIntent in-person (POST /v1/terminal/readers/{reader}/refund_payment). _(POST /api/stripe/terminal/readers/refund/payment/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_set_reader_display_create`

Set reader display (POST /v1/terminal/readers/{reader}/set_reader_display). _(POST /api/stripe/terminal/readers/set/reader/display/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_readers_update`

Update a Reader (POST /v1/terminal/readers/{reader}). _(POST /api/stripe/terminal/readers/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_terminal_refunds_create`

Create a refund using a Terminal-supported device. _(POST /api/stripe/terminal/refunds/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_confirmation_tokens_create`

Create a test Confirmation Token (POST /v1/test_helpers/confirmation_tokens). _(POST /api/stripe/test/helpers/confirmation/tokens/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_customers_fund_cash_balance_create`

Fund a test mode cash balance (POST /v1/test_helpers/customers/{customer}/fund_cash_balance). _(POST /api/stripe/test/helpers/customers/fund/cash/balance/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `customer` | string | Sim | Path param "customer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_authorizations_capture_create`

Capture a test-mode authorization (POST /v1/test_helpers/issuing/authorizations/{authorization}/capture). _(POST /api/stripe/test/helpers/issuing/authorizations/capture/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_authorizations_create`

Create a test-mode authorization (POST /v1/test_helpers/issuing/authorizations). _(POST /api/stripe/test/helpers/issuing/authorizations/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_authorizations_expire_create`

Expire a test-mode authorization (POST /v1/test_helpers/issuing/authorizations/{authorization}/expire). _(POST /api/stripe/test/helpers/issuing/authorizations/expire/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_authorizations_finalize_amount_create`

Finalize a test-mode authorization's amount (POST /v1/test_helpers/issuing/authorizations/{authorization}/finalize_amount). _(POST /api/stripe/test/helpers/issuing/authorizations/finalize/amount/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_authorizations_fraud_challenges_respond_create`

Respond to fraud challenge (POST /v1/test_helpers/issuing/authorizations/{authorization}/fraud_challenges/respond). _(POST /api/stripe/test/helpers/issuing/authorizations/fraud/challenges/respond/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_authorizations_increment_create`

Increment a test-mode authorization (POST /v1/test_helpers/issuing/authorizations/{authorization}/increment). _(POST /api/stripe/test/helpers/issuing/authorizations/increment/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_authorizations_reverse_create`

Reverse a test-mode authorization (POST /v1/test_helpers/issuing/authorizations/{authorization}/reverse). _(POST /api/stripe/test/helpers/issuing/authorizations/reverse/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `authorization` | string | Sim | Path param "authorization" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_cards_shipping_deliver_create`

Deliver a testmode card (POST /v1/test_helpers/issuing/cards/{card}/shipping/deliver). _(POST /api/stripe/test/helpers/issuing/cards/shipping/deliver/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `card` | string | Sim | Path param "card" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_cards_shipping_fail_create`

Fail a testmode card (POST /v1/test_helpers/issuing/cards/{card}/shipping/fail). _(POST /api/stripe/test/helpers/issuing/cards/shipping/fail/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `card` | string | Sim | Path param "card" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_cards_shipping_return_create`

Return a testmode card (POST /v1/test_helpers/issuing/cards/{card}/shipping/return). _(POST /api/stripe/test/helpers/issuing/cards/shipping/return/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `card` | string | Sim | Path param "card" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_cards_shipping_ship_create`

Ship a testmode card (POST /v1/test_helpers/issuing/cards/{card}/shipping/ship). _(POST /api/stripe/test/helpers/issuing/cards/shipping/ship/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `card` | string | Sim | Path param "card" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_cards_shipping_submit_create`

Submit a testmode card (POST /v1/test_helpers/issuing/cards/{card}/shipping/submit). _(POST /api/stripe/test/helpers/issuing/cards/shipping/submit/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `card` | string | Sim | Path param "card" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_personalization_designs_activate_create`

Activate a testmode personalization design (POST /v1/test_helpers/issuing/personalization_designs/{personalization_design}/activate). _(POST /api/stripe/test/helpers/issuing/personalization/designs/activate/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `personalization_design` | string | Sim | Path param "personalization_design" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_personalization_designs_deactivate_create`

Deactivate a testmode personalization design (POST /v1/test_helpers/issuing/personalization_designs/{personalization_design}/deactivate). _(POST /api/stripe/test/helpers/issuing/personalization/designs/deactivate/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `personalization_design` | string | Sim | Path param "personalization_design" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_personalization_designs_reject_create`

Reject a testmode personalization design (POST /v1/test_helpers/issuing/personalization_designs/{personalization_design}/reject). _(POST /api/stripe/test/helpers/issuing/personalization/designs/reject/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `personalization_design` | string | Sim | Path param "personalization_design" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_settlements_complete_create`

Complete a test-mode settlement (POST /v1/test_helpers/issuing/settlements/{settlement}/complete). _(POST /api/stripe/test/helpers/issuing/settlements/complete/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `settlement` | string | Sim | Path param "settlement" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_settlements_create`

Create a test-mode settlement (POST /v1/test_helpers/issuing/settlements). _(POST /api/stripe/test/helpers/issuing/settlements/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_transactions_create_force_capture_create`

Create a test-mode force capture (POST /v1/test_helpers/issuing/transactions/create_force_capture). _(POST /api/stripe/test/helpers/issuing/transactions/create/force/capture/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_transactions_create_unlinked_refund_create`

Create a test-mode unlinked refund (POST /v1/test_helpers/issuing/transactions/create_unlinked_refund). _(POST /api/stripe/test/helpers/issuing/transactions/create/unlinked/refund/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_issuing_transactions_refund_create`

Refund a test-mode transaction (POST /v1/test_helpers/issuing/transactions/{transaction}/refund). _(POST /api/stripe/test/helpers/issuing/transactions/refund/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transaction` | string | Sim | Path param "transaction" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_refunds_expire_create`

Expire a pending refund. (POST /v1/test_helpers/refunds/{refund}/expire). [write, mexe em dinheiro] _(POST /api/stripe/test/helpers/refunds/expire/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `refund` | string | Sim | Path param "refund" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_terminal_readers_present_payment_method_create`

Simulate presenting a payment method (POST /v1/test_helpers/terminal/readers/{reader}/present_payment_method). _(POST /api/stripe/test/helpers/terminal/readers/present/payment/method/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_terminal_readers_succeed_input_collection_create`

Simulate a successful input collection (POST /v1/test_helpers/terminal/readers/{reader}/succeed_input_collection). _(POST /api/stripe/test/helpers/terminal/readers/succeed/input/collection/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_terminal_readers_timeout_input_collection_create`

Simulate an input collection timeout (POST /v1/test_helpers/terminal/readers/{reader}/timeout_input_collection). _(POST /api/stripe/test/helpers/terminal/readers/timeout/input/collection/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `reader` | string | Sim | Path param "reader" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_test_clocks_advance_create`

Advance a test clock (POST /v1/test_helpers/test_clocks/{test_clock}/advance). _(POST /api/stripe/test/helpers/test/clocks/advance/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `test_clock` | string | Sim | Path param "test_clock" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_test_clocks_create`

Create a test clock (POST /v1/test_helpers/test_clocks). _(POST /api/stripe/test/helpers/test/clocks/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_test_clocks_delete`

Delete a test clock (DELETE /v1/test_helpers/test_clocks/{test_clock}). _(POST /api/stripe/test/helpers/test/clocks/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `test_clock` | string | Sim | Path param "test_clock" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_test_clocks_get`

Retrieve a test clock (GET /v1/test_helpers/test_clocks/{test_clock}). _(POST /api/stripe/test/helpers/test/clocks/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `test_clock` | string | Sim | Path param "test_clock" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_test_helpers_test_clocks_list`

List all test clocks (GET /v1/test_helpers/test_clocks). _(POST /api/stripe/test/helpers/test/clocks/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_test_helpers_treasury_inbound_transfers_fail_create`

Test mode: Fail an InboundTransfer (POST /v1/test_helpers/treasury/inbound_transfers/{id}/fail). _(POST /api/stripe/test/helpers/treasury/inbound/transfers/fail/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_test_helpers_treasury_inbound_transfers_return_create`

Test mode: Return an InboundTransfer (POST /v1/test_helpers/treasury/inbound_transfers/{id}/return). _(POST /api/stripe/test/helpers/treasury/inbound/transfers/return/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_test_helpers_treasury_inbound_transfers_succeed_create`

Test mode: Succeed an InboundTransfer (POST /v1/test_helpers/treasury/inbound_transfers/{id}/succeed). _(POST /api/stripe/test/helpers/treasury/inbound/transfers/succeed/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_test_helpers_treasury_outbound_payments_fail_create`

Test mode: Fail an OutboundPayment (POST /v1/test_helpers/treasury/outbound_payments/{id}/fail). _(POST /api/stripe/test/helpers/treasury/outbound/payments/fail/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_test_helpers_treasury_outbound_payments_post_create`

Test mode: Post an OutboundPayment (POST /v1/test_helpers/treasury/outbound_payments/{id}/post). _(POST /api/stripe/test/helpers/treasury/outbound/payments/post/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_test_helpers_treasury_outbound_payments_return_create`

Test mode: Return an OutboundPayment (POST /v1/test_helpers/treasury/outbound_payments/{id}/return). _(POST /api/stripe/test/helpers/treasury/outbound/payments/return/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_test_helpers_treasury_outbound_payments_update`

Test mode: Update an OutboundPayment (POST /v1/test_helpers/treasury/outbound_payments/{id}). _(POST /api/stripe/test/helpers/treasury/outbound/payments/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_test_helpers_treasury_outbound_transfers_fail_create`

Test mode: Fail an OutboundTransfer (POST /v1/test_helpers/treasury/outbound_transfers/{outbound_transfer}/fail). _(POST /api/stripe/test/helpers/treasury/outbound/transfers/fail/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `outbound_transfer` | string | Sim | Path param "outbound_transfer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_treasury_outbound_transfers_post_create`

Test mode: Post an OutboundTransfer (POST /v1/test_helpers/treasury/outbound_transfers/{outbound_transfer}/post). _(POST /api/stripe/test/helpers/treasury/outbound/transfers/post/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `outbound_transfer` | string | Sim | Path param "outbound_transfer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_treasury_outbound_transfers_return_create`

Test mode: Return an OutboundTransfer (POST /v1/test_helpers/treasury/outbound_transfers/{outbound_transfer}/return). _(POST /api/stripe/test/helpers/treasury/outbound/transfers/return/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `outbound_transfer` | string | Sim | Path param "outbound_transfer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_treasury_outbound_transfers_update`

Test mode: Update an OutboundTransfer (POST /v1/test_helpers/treasury/outbound_transfers/{outbound_transfer}). _(POST /api/stripe/test/helpers/treasury/outbound/transfers/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `outbound_transfer` | string | Sim | Path param "outbound_transfer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_treasury_received_credits_create`

Test mode: Create a ReceivedCredit (POST /v1/test_helpers/treasury/received_credits). _(POST /api/stripe/test/helpers/treasury/received/credits/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_test_helpers_treasury_received_debits_create`

Test mode: Create a ReceivedDebit (POST /v1/test_helpers/treasury/received_debits). _(POST /api/stripe/test/helpers/treasury/received/debits/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tokens_create`

Create a bank account token (POST /v1/tokens). _(POST /api/stripe/tokens/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_tokens_get`

Retrieve a token (GET /v1/tokens/{token}). _(POST /api/stripe/tokens/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `token` | string | Sim | Path param "token" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_topups_cancel_create`

Cancel a top-up (POST /v1/topups/{topup}/cancel). _(POST /api/stripe/topups/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `topup` | string | Sim | Path param "topup" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_topups_create`

Create a top-up (POST /v1/topups). _(POST /api/stripe/topups/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_topups_get`

Retrieve a top-up (GET /v1/topups/{topup}). _(POST /api/stripe/topups/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `topup` | string | Sim | Path param "topup" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_topups_list`

List all top-ups (GET /v1/topups). _(POST /api/stripe/topups/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_topups_update`

Update a top-up (POST /v1/topups/{topup}). _(POST /api/stripe/topups/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `topup` | string | Sim | Path param "topup" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_transfers_create`

Create a transfer (POST /v1/transfers). _(POST /api/stripe/transfers/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_transfers_get`

Retrieve a transfer (GET /v1/transfers/{transfer}). _(POST /api/stripe/transfers/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transfer` | string | Sim | Path param "transfer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_transfers_list`

List all transfers (GET /v1/transfers). _(POST /api/stripe/transfers/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_transfers_reversals_create`

Create a transfer reversal (POST /v1/transfers/{id}/reversals). _(POST /api/stripe/transfers/reversals/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_transfers_reversals_get`

Retrieve a reversal (GET /v1/transfers/{transfer}/reversals/{id}). _(POST /api/stripe/transfers/reversals/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transfer` | string | Sim | Path param "transfer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_transfers_reversals_list`

List all reversals (GET /v1/transfers/{id}/reversals). _(POST /api/stripe/transfers/reversals/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_transfers_reversals_update`

Update a reversal (POST /v1/transfers/{transfer}/reversals/{id}). _(POST /api/stripe/transfers/reversals/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transfer` | string | Sim | Path param "transfer" (obrigatório) |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_transfers_update`

Update a transfer (POST /v1/transfers/{transfer}). _(POST /api/stripe/transfers/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `transfer` | string | Sim | Path param "transfer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_credit_reversals_create`

Create a CreditReversal (POST /v1/treasury/credit_reversals). _(POST /api/stripe/treasury/credit/reversals/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_credit_reversals_get`

Retrieve a CreditReversal (GET /v1/treasury/credit_reversals/{credit_reversal}). _(POST /api/stripe/treasury/credit/reversals/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `credit_reversal` | string | Sim | Path param "credit_reversal" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_credit_reversals_list`

List all CreditReversals (GET /v1/treasury/credit_reversals). _(POST /api/stripe/treasury/credit/reversals/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_debit_reversals_create`

Create a DebitReversal (POST /v1/treasury/debit_reversals). _(POST /api/stripe/treasury/debit/reversals/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_debit_reversals_get`

Retrieve a DebitReversal (GET /v1/treasury/debit_reversals/{debit_reversal}). _(POST /api/stripe/treasury/debit/reversals/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `debit_reversal` | string | Sim | Path param "debit_reversal" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_debit_reversals_list`

List all DebitReversals (GET /v1/treasury/debit_reversals). _(POST /api/stripe/treasury/debit/reversals/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_financial_accounts_close_create`

Close a FinancialAccount (POST /v1/treasury/financial_accounts/{financial_account}/close). _(POST /api/stripe/treasury/financial/accounts/close/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `financial_account` | string | Sim | Path param "financial_account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_financial_accounts_create`

Create a FinancialAccount (POST /v1/treasury/financial_accounts). _(POST /api/stripe/treasury/financial/accounts/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_financial_accounts_features_create`

Update FinancialAccount Features (POST /v1/treasury/financial_accounts/{financial_account}/features). _(POST /api/stripe/treasury/financial/accounts/features/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `financial_account` | string | Sim | Path param "financial_account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_financial_accounts_features_list`

Retrieve FinancialAccount Features (GET /v1/treasury/financial_accounts/{financial_account}/features). _(POST /api/stripe/treasury/financial/accounts/features/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `financial_account` | string | Sim | Path param "financial_account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_financial_accounts_get`

Retrieve a FinancialAccount (GET /v1/treasury/financial_accounts/{financial_account}). _(POST /api/stripe/treasury/financial/accounts/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `financial_account` | string | Sim | Path param "financial_account" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_financial_accounts_list`

List all FinancialAccounts (GET /v1/treasury/financial_accounts). _(POST /api/stripe/treasury/financial/accounts/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_financial_accounts_update`

Update a FinancialAccount (POST /v1/treasury/financial_accounts/{financial_account}). _(POST /api/stripe/treasury/financial/accounts/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `financial_account` | string | Sim | Path param "financial_account" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_inbound_transfers_cancel_create`

Cancel an InboundTransfer (POST /v1/treasury/inbound_transfers/{inbound_transfer}/cancel). _(POST /api/stripe/treasury/inbound/transfers/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `inbound_transfer` | string | Sim | Path param "inbound_transfer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_inbound_transfers_create`

Create an InboundTransfer (POST /v1/treasury/inbound_transfers). _(POST /api/stripe/treasury/inbound/transfers/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_inbound_transfers_get`

Retrieve an InboundTransfer (GET /v1/treasury/inbound_transfers/{id}). _(POST /api/stripe/treasury/inbound/transfers/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_treasury_inbound_transfers_list`

List all InboundTransfers (GET /v1/treasury/inbound_transfers). _(POST /api/stripe/treasury/inbound/transfers/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_outbound_payments_cancel_create`

Cancel an OutboundPayment (POST /v1/treasury/outbound_payments/{id}/cancel). _(POST /api/stripe/treasury/outbound/payments/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_treasury_outbound_payments_create`

Create an OutboundPayment (POST /v1/treasury/outbound_payments). _(POST /api/stripe/treasury/outbound/payments/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_outbound_payments_get`

Retrieve an OutboundPayment (GET /v1/treasury/outbound_payments/{id}). _(POST /api/stripe/treasury/outbound/payments/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_treasury_outbound_payments_list`

List all OutboundPayments (GET /v1/treasury/outbound_payments). _(POST /api/stripe/treasury/outbound/payments/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_outbound_transfers_cancel_create`

Cancel an OutboundTransfer (POST /v1/treasury/outbound_transfers/{outbound_transfer}/cancel). _(POST /api/stripe/treasury/outbound/transfers/cancel/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `outbound_transfer` | string | Sim | Path param "outbound_transfer" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_outbound_transfers_create`

Create an OutboundTransfer (POST /v1/treasury/outbound_transfers). _(POST /api/stripe/treasury/outbound/transfers/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_treasury_outbound_transfers_get`

Retrieve an OutboundTransfer (GET /v1/treasury/outbound_transfers/{outbound_transfer}). _(POST /api/stripe/treasury/outbound/transfers/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `outbound_transfer` | string | Sim | Path param "outbound_transfer" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_outbound_transfers_list`

List all OutboundTransfers (GET /v1/treasury/outbound_transfers). _(POST /api/stripe/treasury/outbound/transfers/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_received_credits_get`

Retrieve a ReceivedCredit (GET /v1/treasury/received_credits/{id}). _(POST /api/stripe/treasury/received/credits/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_treasury_received_credits_list`

List all ReceivedCredits (GET /v1/treasury/received_credits). _(POST /api/stripe/treasury/received/credits/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_received_debits_get`

Retrieve a ReceivedDebit (GET /v1/treasury/received_debits/{id}). _(POST /api/stripe/treasury/received/debits/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_treasury_received_debits_list`

List all ReceivedDebits (GET /v1/treasury/received_debits). _(POST /api/stripe/treasury/received/debits/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_transaction_entries_get`

Retrieve a TransactionEntry (GET /v1/treasury/transaction_entries/{id}). _(POST /api/stripe/treasury/transaction/entries/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_treasury_transaction_entries_list`

List all TransactionEntries (GET /v1/treasury/transaction_entries). _(POST /api/stripe/treasury/transaction/entries/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_treasury_transactions_get`

Retrieve a Transaction (GET /v1/treasury/transactions/{id}). _(POST /api/stripe/treasury/transactions/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `id` | string | Sim | Path param "id" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `stripe_treasury_transactions_list`

List all Transactions (GET /v1/treasury/transactions). _(POST /api/stripe/treasury/transactions/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_webhook_endpoints_create`

Create a webhook endpoint (POST /v1/webhook_endpoints). _(POST /api/stripe/webhook/endpoints/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_webhook_endpoints_delete`

Delete a webhook endpoint (DELETE /v1/webhook_endpoints/{webhook_endpoint}). _(POST /api/stripe/webhook/endpoints/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `webhook_endpoint` | string | Sim | Path param "webhook_endpoint" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

#### `stripe_webhook_endpoints_get`

Retrieve a webhook endpoint (GET /v1/webhook_endpoints/{webhook_endpoint}). _(POST /api/stripe/webhook/endpoints/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `webhook_endpoint` | string | Sim | Path param "webhook_endpoint" (obrigatório) |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_webhook_endpoints_list`

List all webhook endpoints (GET /v1/webhook_endpoints). _(POST /api/stripe/webhook/endpoints/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `query` | string | Não | Query params como JSON string OU querystring (filtros do recurso + paginação limit (máx 100)/starting_after/ending_before + expand). Ex.: {"limit":10,"status":"open"} ou limit=10&status=open. Objetos/arrays aninhados são form-encoded (colchetes) pelo adapter, ex.: {"created":{"gte":1690000000},"expand":["customer"]}. Campos em stripe.com/docs/api. |

#### `stripe_webhook_endpoints_update`

Update a webhook endpoint (POST /v1/webhook_endpoints/{webhook_endpoint}). _(POST /api/stripe/webhook/endpoints/update)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Quando há múltiplas contas Stripe conectadas: id ou label da conexão. Veja stripe_list_accounts. |
| `webhook_endpoint` | string | Sim | Path param "webhook_endpoint" (obrigatório) |
| `body` | string | Não | Corpo da requisição como JSON string (campos do recurso). O adapter serializa pro form-urlencoded (colchetes deep) que o Stripe espera. Ex. criar customer: {"email":"a@b.com","metadata":{"order_id":"42"}}. Ex. criar payment_intent: {"amount":2000,"currency":"usd","payment_method_types":["card"]}. Campos em stripe.com/docs/api. |
| `idempotency_key` | string | Não | Idempotency-Key opcional (header): repetir a mesma chave garante que a operação só acontece uma vez (retry seguro em criação de cobrança/pagamento). |

---

Este MCP também funciona via **conexão MCP** (Claude / Cursor) em `https://api.mcp.ai/p_stripe` — veja o [README](../../README.md). A skill acima é pra consumir a **REST API** direto (agente próprio / código).
