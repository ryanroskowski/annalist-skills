# Annalist agent skill

Lets an AI agent find and use [Annalist](https://annalist.dev): signed, pay-per-call data on Hyperliquid.

- crowd state, with what usually followed
- smart-money positioning
- liquidation levels against order-book depth
- builder-deployed stock, index, commodity and FX perps
- prediction-market odds
- the HLP vault

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

- catalog
- sample
- base rates
- track record

## Trust

- Every paid answer is signed with Ed25519 (key at `https://api.annalist.dev/.well-known/annalist-key.json`).
- The archive behind the answers is append-only.
- Its daily hashes are published in [annalist-ledger](https://github.com/ryanroskowski/annalist-ledger) and
  timestamped in Bitcoin.

Data, not investment advice.

This skill file is generated from Annalist's route catalog; open issues here.
