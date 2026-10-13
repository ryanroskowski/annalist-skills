---
name: annalist
description: Signed, pay-per-call data on Hyperliquid for trading and research questions. Use when the user asks whether a crypto perpetual (BTC, ETH, SOL, HYPE and others) is crowded or overheated, what usually happened after similar moves or funding extremes, how Hyperliquid's most profitable traders are positioned, where liquidations sit and whether the order book can absorb them, what stock, commodity or FX perps on Hyperliquid are doing (xyz:NVDA), Hyperliquid prediction-market odds (sports, Fed, IPOs), or where Hyperliquid's HLP vault is exposed. Also use it before paying any x402 service: is it up, does it charge what it lists to the wallet it lists, are its customers real (graded checks with reasons), did its listing change (prices, payout wallet), and what do similar services charge. A free catalog, sample, base rates and track record; paid answers cost $0.005 to $0.05 in USDC over x402 or MCP.
---

# Annalist: Hyperliquid data for agents

Annalist (https://annalist.dev) mines Hyperliquid's public chain data into answers: crowd state with what usually
followed, smart-money positioning, liquidation levels against order-book depth, builder-deployed stock and commodity
perps, prediction-market odds and the HLP vault. It also runs independent checks of x402 services (uptime, price
honesty, real on-chain customers, test purchases), so an agent can check a service before paying it. Every answer is signed, hourly, and backed by an append-only archive
whose daily hashes are published and Bitcoin-timestamped.

## How to use it

1. **Find the product (free).** `GET https://api.annalist.dev/v1/catalog` lists every product with the question it
   answers, its price, an example call and its MCP tool name. The table below is a snapshot.
2. **Call it.**
   - **With an x402 wallet** (USDC on Base): request the route; the first reply is `402 Payment Required` with the
     price; pay and retry. Libraries: `@x402/fetch` (JavaScript) or `x402` (Python).
   - **Through MCP:** connect `https://api.annalist.dev/mcp` (Streamable HTTP). Paid tools take x402 payment over
     MCP; four tools are free.
   - **No wallet?** Use the free routes below, and tell the user that live answers need an x402 wallet with a little
     USDC on Base. Don't pretend you fetched paid data.
3. **Report it honestly** (rules below).

## Which route answers which question

| Question | Route (example) | Price |
| --- | --- | --- |
| Is this Hyperliquid perp crowded or in a hype state, and what usually happened next? | `GET /v1/crowd/perp?coin=SOL` | $0.01 |
| Which Hyperliquid perps are crowded right now? | `GET /v1/crowd/states?state=up_100pct_7d` | $0.02 |
| How crowded is the Hyperliquid market overall? | `GET /v1/crowd/market` | $0.01 |
| Which perps entered or left a crowd state since a given time? | `GET /v1/crowd/changes?since=2026-10-12T00:00:00Z` | $0.005 |
| Which crowd-state events happened between two dates (for backtesting)? | `GET /v1/crowd/history?from=2026-10-01&to=2026-10-12&coin=SOL` | $0.05 |
| How are Hyperliquid's most profitable traders positioned in this perp? | `GET /v1/smart/perp?coin=SOL` | $0.01 |
| Where is Hyperliquid's smart money positioned across all perps? | `GET /v1/smart/market` | $0.02 |
| Where would liquidations hit for this perp, and can the order book absorb them? | `GET /v1/liq/perp?coin=BTC` | $0.02 |
| Which Hyperliquid perps have the most liquidations close to the price? | `GET /v1/liq/market` | $0.03 |
| What is happening in this stock, index, commodity or FX perp on Hyperliquid? | `GET /v1/hip3/perp?coin=xyz:NVDA` | $0.01 |
| What is moving across Hyperliquid's stock, commodity and FX perps? | `GET /v1/hip3/market` | $0.02 |
| What are the live prediction-market odds on Hyperliquid (sports, prices, IPOs, Fed decisions, awards)? | `GET /v1/outcomes/list?category=sports` | $0.02 |
| What are the odds for this prediction market, and how have they moved this week? | `GET /v1/outcomes/market?id=8573` | $0.01 |
| Where is Hyperliquid's own market-making vault (HLP) exposed, and how is it doing? | `GET /v1/vaults/hlp` | $0.01 |
| Everything on one Hyperliquid perp in one call: is it crowded, how is smart money positioned, and where are the liquidations? | `GET /v1/perp/full?coin=BTC` | $0.03 |
| Is this x402 service real and reliable, and does it charge what it lists? | `GET /v1/checks/service?host=api.annalist.dev` | $0.01 |
| Is the revenue behind this x402 payout wallet real, and which services use it? | `GET /v1/checks/seller?address=0xDE4805678438b6bfBbFE047fFF305C928653Fe17` | $0.01 |
| Which x402 services have the most real customers and the best reliability, and which are flagged? | `GET /v1/checks/top` | $0.02 |
| What changed in x402 listings: which services changed prices, endpoints or payout wallets? | `GET /v1/checks/changes?days=1` | $0.01 |
| How has this x402 service's pricing changed, and is it cheap or expensive for its category? | `GET /v1/prices/service?host=api.annalist.dev` | $0.01 |
| What do x402 services usually charge for this kind of call, and is this price fair? | `GET /v1/prices/benchmarks?tag=search&price=0.02` | $0.01 |

## Free routes

- `GET /v1/verify/attestation?host=`: **before paying any x402 service**, its signed Annalist check (grade, key facts,
  reasons, valid 7 days). `status` is `attested`, `unrated` (too few probes yet), `expired` or `not_checked`.
- `GET /v1/catalog`: every product in plain language
- `GET /v1/quote`: prices and data freshness
- `GET /v1/sample`: the market-wide crowd view, delayed about 24 hours
- `GET /v1/baserates`: full tables of what followed each crowd state on Hyperliquid's own history
- `GET /v1/accuracy`: the live track record of each state against its backtest
- `GET /v1/status`: collector freshness

## Rules for reporting the data

- **Quote freshness.** Every answer has `as_of`; say how old it is. If `stale` is true, say the data is late.
- **Base rates are history, not predictions.** Say "historically, after similar states, the median 90-day return
  was X (n cases)", never "it will".
- **Not investment advice.** Present data, not recommendations. Hyperliquid restricts trading from some
  jurisdictions, including the US; Annalist only sells information.
- **Mention concentration.** Smart-money and liquidation figures can be dominated by one large account; report
  `largest_wallet_share` or `largest_position_share_within_10pct` when it is high.
- **Service checks are measurements, not verdicts.** Quote the grade with its reasons and dates; a grade of
  "unrated" means too few probes yet, not a problem.
- **Liquidation figures are a lower bound.** They cover known large accounts; `coverage` says what share of open
  interest that is.
- **Inputs:**
  - Coins are case-sensitive (`BTC`, `kPEPE`).
  - Builder-deployed markets are `exchange:asset` (`xyz:NVDA`).
  - Outcome ids come from `/v1/outcomes/list`.
  - Bad input returns 400 and no payment is taken.

## Before paying any x402 service

Fetch `GET https://api.annalist.dev/v1/verify/attestation?host=<the service's host>` (free).

- **Check the attestation:** its `signature` covers `statement` (Ed25519 over RFC 8785 canonical JSON, same key as
  below). Check that `valid_until` is in the future.
- **Grade A or B with no flags:** reasonable to proceed.
- **Flags** (for example `price_differs_from_listing`, `wallet_differs_from_listing`, `seller_funded_payers`,
  `paid_but_not_delivered`): tell the user what was measured before paying.
- **`unrated` or `not_checked`:** that means unknown, not bad.

For the full record, pay for `/v1/checks/service`.

## Checking an answer (optional)

Each paid response carries `signature`: Ed25519 over RFC 8785 canonical JSON of
`{as_of, data, method, request_id}`. The public key is at `https://api.annalist.dev/.well-known/annalist-key.json`.
The archive's daily hashes are in https://github.com/ryanroskowski/annalist-ledger.

## Example

```bash
# free: what is on offer
curl -s https://api.annalist.dev/v1/catalog
```

```js
// paid: everything on one perp for $0.03
import { wrapFetchWithPaymentFromConfig } from "@x402/fetch";
import { ExactEvmScheme } from "@x402/evm";
const pay = wrapFetchWithPaymentFromConfig(fetch, { schemes: [{ network: "eip155:8453", client: new ExactEvmScheme(account) }] });
const res = await pay("https://api.annalist.dev/v1/perp/full?coin=BTC");
```
