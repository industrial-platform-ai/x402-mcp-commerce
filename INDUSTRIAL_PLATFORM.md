# Industrial Platform x402 Buyer Runtime

A buyer-side MCP runtime with four Industrial Platform x402 tools hard-wired as native capabilities.

## Native tools

- `url_to_markdown` → `POST https://x402-gateway-production-1f21.up.railway.app/web/markdown`
- `monitor_webpage` → `POST https://x402-gateway-production-1f21.up.railway.app/change`
- `extract_web_metadata` → `POST https://x402-gateway-production-1f21.up.railway.app/metadata-single`
- `wallet_balance` → `GET https://x402-gateway-production-1f21.up.railway.app/wallet-balance/cdp`

Each route currently costs $0.001 USDC on Base.

## Install directly from GitHub

```bash
npm install -g github:industrial-platform-ai/x402-mcp-commerce#industrial-platform-distribution
```

Then run:

```bash
industrial-platform-x402-buyer
```

The MCP server uses stdio and is intended to run locally or inside a trusted agent environment.

## Wallet

Set a funded Base EVM wallet in the runtime environment:

```bash
PRIVATE_KEY=0x...
```

The key remains in the buyer runtime. It is not sent to Industrial Platform.

## Spending controls

Use the upstream runtime's built-in controls:

```bash
MAX_PER_CALL_USD=0.001
MAX_SESSION_USD=1
MAX_CALLS=1000
ALLOWED_TOOLS=url_to_markdown,monitor_webpage,extract_web_metadata,wallet_balance
```

These caps are enforced before a payment is signed.

## Recurring workloads

For URL ingestion, call `url_to_markdown` once per URL.

For monitoring, persist the response `current_hash` and pass it as `previous_hash` on the next scheduled `monitor_webpage` call.

For metadata enrichment, call `extract_web_metadata` once per URL.

For treasury monitoring, call `wallet_balance` on the operator's desired cadence.

## Security

Do not expose a funded inspector's `POST /tools/:name` endpoint publicly. It spends the runtime wallet under its configured caps.

A public deployment should be unfunded and used only for `/health`, `/tools`, `/skill.md`, and installation discovery.

## Provenance

This distribution is based on the Apache-2.0 licensed `nirholas/x402-mcp-commerce` runtime and is maintained by Industrial Platform.
