# Annalist agent skill

Lets an AI agent find and use [Annalist](https://annalist.dev): signed, pay-per-call data for agents, in four areas.

- **Hyperliquid:**
  - crowd state, with what usually followed
  - smart-money positioning
  - liquidation levels against order-book depth
  - builder-deployed stock, index, commodity and FX perps
  - prediction-market odds
  - the HLP vault
- **Checks before an agent pays or connects:**
  - x402 services: uptime, whether they charge what they list, real on-chain customers with fake volume removed,
    sanctioned payout wallets, weekly test purchases, price history and listing changes
  - remote MCP servers from the Official MCP Registry: uptime, sign-in mode and tool changes
- **Software supply chain:** the most-used npm, PyPI, Rust and Go packages, watched daily for takeover patterns. These
  are new publisher accounts, maintainer and owner changes, new install scripts, dropped provenance and moved source
  repositories.
- **AI model prices:** cost per million tokens across providers for about 4,000 models, with history since January
  2026.

The skill tells the agent when to reach for Annalist, which route answers which question, and how to report the data
honestly. The live list of products is always at `https://api.annalist.dev/v1/catalog` (free).

## Install

**Claude Code** (adds the skill and the MCP server):

```
/plugin marketplace add ryanroskowski/annalist-skills
/plugin install annalist@annalist
```

**Skills CLI** (works with many agents):

```
npx skills add ryanroskowski/annalist-skills
```

**Any agent that reads Agent Skills** (Codex, Muse and others): copy `skills/annalist/` into the agent's skills
folder, for example:

- `~/.codex/skills/annalist/` for Codex
- `~/workspace/skills/annalist/` for Muse

**MCP only:** connect `https://api.annalist.dev/mcp` (Streamable HTTP).

## Paying

Paid answers cost $0.005 to $0.05 in USDC on Base. They are paid per call over [x402](https://x402.org), directly or
through MCP. There are no accounts and no API keys.

Agents without an x402 wallet can still use the free routes:

- `/v1/catalog`: every product, with its price
- `/v1/verify/attestation?host=`: an x402 service's signed check before paying it
- `/v1/verify/mcp?url=`: an MCP server's signed check before connecting it
- `/v1/report`: the Annalist weekly, the agent economy this week
- `/v1/quote` and `/v1/status`: prices and freshness
- `/v1/sample`, `/v1/baserates` and `/v1/accuracy`: Hyperliquid samples, base rates and track record

## Trust

- Every paid answer is signed with Ed25519 (key at `https://api.annalist.dev/.well-known/annalist-key.json`).
- The archive behind the answers is append-only. Its daily hashes are published in
  [annalist-ledger](https://github.com/ryanroskowski/annalist-ledger) and timestamped in Bitcoin.
- Rankings are never for sale, and Annalist's own service is left out of them.
- A signature proves who issued an answer and that it wasn't changed, not that every fact in it is right; the
  measurements behind it are archived.

Data, not investment advice.

This skill file is generated from Annalist's route catalog; open issues here.
