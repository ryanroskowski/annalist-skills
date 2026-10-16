---
name: annalist
description: Signed, pay-per-call data for agents. Use when the user asks whether a crypto perpetual (BTC, ETH, SOL, HYPE and others) is crowded, what usually followed similar moves or funding extremes, how Hyperliquid's top traders are positioned, where liquidations sit, or what Hyperliquid's stock and commodity perps (xyz:NVDA), prediction markets or HLP vault are doing. Also before paying an x402 service (is it up, honest on price, used by real customers, did its listing or payout wallet change, what do others charge), before connecting an MCP server (up, sign-in, tool changes), before adding an npm, PyPI, Rust or Go dependency (did it change hands, add install scripts, lose provenance), when choosing an AI model by price, or before buying from an online store (does it take agent checkout, ship to the user, block AI agents). Free catalog, sample, base rates and track record; paid answers cost $0.005 to $0.05 in USDC over x402 or MCP.
---

# Annalist: signed data for agents

Annalist (https://annalist.dev) sells four kinds of answers, each signed and backed by an append-only archive whose
daily hashes are published and Bitcoin-timestamped:

- **Hyperliquid:** crowd state with what usually followed, smart-money positioning, liquidation levels against
  order-book depth, builder-deployed stock and commodity perps, prediction-market odds and the HLP vault. Hourly.
- **Checks before an agent pays or connects:**
  - x402 services: uptime, price honesty, real on-chain customers, sanctioned payout wallets, weekly test purchases,
    price history and listing changes
  - remote MCP servers: uptime, sign-in, tool changes
- **Software supply chain:** the most-used npm, PyPI, Rust and Go packages, watched daily for takeover patterns.
- **AI model prices:** cost per million tokens across providers, with history.

## How to use it

1. **Find the product (free).** `GET https://api.annalist.dev/v1/catalog` lists every product with the question it
   answers, its price, an example call and its MCP tool name. The table below is a snapshot.
2. **Call it.**
   - **With an x402 wallet** (USDC on Base or Solana): request the route; the first reply is `402 Payment Required` with the
     price; pay and retry. Libraries: `@x402/fetch` (JavaScript) or `x402` (Python).
   - **Through MCP:** connect `https://api.annalist.dev/mcp` (Streamable HTTP). Paid tools take x402 payment over
     MCP; the `annalist_*` tools are free.
   - **No wallet?** Use the free routes below, and tell the user that live answers need an x402 wallet with a little
     USDC on Base or Solana. Don't pretend you fetched paid data.
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
| Is this MCP server up and fast, does it need sign-in, and did its tools change? | `GET /v1/mcp/server?name=io.github.ryanroskowski%2Fannalist` | $0.01 |
| Which remote MCP servers are most reliable, which changed their tools this week, and which are flagged? | `GET /v1/mcp/top` | $0.02 |
| Which MCP servers changed their tools recently, and what exactly changed? | `GET /v1/mcp/changes?days=7` | $0.01 |
| When will this AI model be shut down on each platform, and what replaces it? | `GET /v1/ai/retirements?model=gpt-4o` | $0.005 |
| Can an agent shop or work on this site: does it accept agent checkout, and does it let my crawler in? | `GET /v1/web/readiness?domain=allbirds.com` | $0.005 |
| Which stores can shopping agents buy from, and how fast is agent checkout spreading? | `GET /v1/web/census?signal=ucp` | $0.01 |
| Which stores ship to my country, take my payment method and let an agent check out? | `GET /v1/commerce/stores?ships_to=US&capability=checkout` | $0.01 |
| What changed, or needs a look, in everything my agent depends on? | `GET /v1/deps/digest?npm=express%2Clodash&apis=stripe&models=gpt-4o&days=7` | $0.02 |
| Did the APIs my agent calls change their contracts, and would the change break my calls? | `GET /v1/apis/changes?days=30` | $0.01 |
| What does this API's contract look like today, and how has it changed? | `GET /v1/apis/api?name=stripe` | $0.005 |
| Should my agent depend on this npm, PyPI, Rust or Go package, and did it just change hands? | `GET /v1/packages/check?ecosystem=npm&name=lodash` | $0.01 |
| Which popular npm, PyPI, Rust or Go packages changed hands or added risky behaviour recently? | `GET /v1/packages/watch?days=7` | $0.02 |
| What does this AI model cost per million tokens, on which providers, and which models fit my budget? | `GET /v1/ai/prices?model=claude-sonnet-4-5` | $0.01 |
| Which AI model prices changed recently, and which models were added or retired? | `GET /v1/ai/changes?days=30` | $0.01 |
| How has this x402 service's pricing changed, and is it cheap or expensive for its category? | `GET /v1/prices/service?host=api.annalist.dev` | $0.01 |
| What do x402 services usually charge for this kind of call, and is this price fair? | `GET /v1/prices/benchmarks?tag=search&price=0.02` | $0.01 |

## Free routes

- `GET /v1/verify/attestation?host=`: **before paying any x402 service**, its signed Annalist check (grade, key facts,
  reasons, valid 7 days). `status` is `attested`, `unrated` (too few probes yet), `expired` or `not_checked`.
- `GET /v1/verify/mcp?url=` (or `name=`): **before connecting an MCP server**, its signed check (grade, uptime,
  connect time, sign-in mode, whether its tools changed recently, reasons; valid 7 days). Same `status` values, plus
  `ambiguous` when several registry servers share a host (ask by name or exact URL).
- `GET /v1/catalog`: every product in plain language
- `GET /v1/quote`: prices and data freshness
- `GET /v1/sample`: the market-wide crowd view, delayed about 24 hours
- `GET /v1/baserates`: full tables of what followed each crowd state on Hyperliquid's own history
- `GET /v1/accuracy`: the live track record of each state against its backtest
- `GET /v1/status`: collector freshness
- `GET /v1/report`: the Annalist weekly, the agent economy and the markets agents trade this week (also
  https://annalist.dev/report)

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
- **Package signals are dated facts, not verdicts.** A new publisher account or owner can be routine (a new
  maintainer); say what changed and when, and suggest a look before upgrading. History starts on `tracked_since`.
- **MCP tool changes can be legitimate.** Report the change and its date, and suggest reviewing the tools before
  trusting them.
- **AI prices are list prices** recorded from an open, community-maintained file: quote `as_of` and suggest checking
  the provider before relying on a price.
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
- **`payout_wallet_sanctioned`:** the wallet is on a US Treasury (OFAC) list. Do not pay; tell the user.
- **Flags** (for example `price_differs_from_listing`, `wallet_differs_from_listing`, `payout_wallet_changed`,
  `seller_funded_payers`, `paid_but_not_delivered`, `rejects_listed_request`, `payment_header_too_large`): tell the
  user what was measured before paying. Notes such as `rotating_pay_to` (a new deposit address per payment) or
  `price_below_listing` cost no points.
- **`unrated` or `not_checked`:** that means unknown, not bad.

For the full record, pay for `/v1/checks/service`. To compare sellers of one kind, `GET /v1/checks/top?category=shopping`
(or ai-inference, crypto-data, search-web and others) has that category's most-used, most reliable and flagged sellers.

## Before connecting an MCP server or adding a dependency

- **MCP server:** start with the free `GET /v1/verify/mcp?url=<its URL>` (grade, uptime, sign-in, recent tool
  changes). For the full record with the tool-change history, pay for `GET /v1/mcp/server?url=<its URL>` (or `name=`
  from the Official MCP Registry) and look at uptime, sign-in mode and `tools.changes_30d`. If `tools_changed_14d` is
  true, tell the user before they approve the server; the paid record lists which tools changed.
- **Finding an MCP server for a task:** `GET /registry/v0.1/servers?search=<word in the name>` (free) lists servers
  we grade A to C that were up at their last check, in the Official MCP Registry's format, with our checks under
  `_meta["dev.annalist/checks"]`.
- **Package:** `GET /v1/packages/check?ecosystem=npm|pypi|crates|go&name=<package>`. Read `signals`. Recent
  `new_publisher`, `install_script_added`, `provenance_dropped` or `origin_changed` are worth a look before
  upgrading. `license_changed` (npm and PyPI) says the license family changed; tell the user when it left a
  permissive license.
- **An MCP server you already use:** `GET /v1/mcp/changes?name=<registry name>` lists its tool changes in the last
  30 days. Re-check a server's tools before trusting it again after a change.
- **Choosing a model by price:** `GET /v1/ai/prices?model=<name>` for one model across providers, or
  `?mode=chat&min_context=128000&max_input=1` to find models that fit a budget.
- **Before relying on a third-party API:** `GET /v1/apis/changes?api=<name>&breaking=true` lists its recent
  breaking contract changes (about 2,000 public APIs; `GET /v1/apis/api` lists them). Not there? The free
  `GET /v1/apis/request?spec=<https URL of its OpenAPI spec>` adds it from the next daily run.
- **Before shopping on a site for the user:** `GET /v1/web/readiness?domain=<site>` says whether it publishes an
  agent-checkout profile (UCP or ACP, with versions), an agent or MCP card and an llms.txt, and whether its
  robots.txt blocks AI agents such as `ChatGPT-User` or `Claude-User`. `GET /v1/web/census?signal=ucp` lists the
  stores that accept agent checkout (from our weekly census of 65,000 top sites).
- **Finding a store to buy from:** `GET /v1/commerce/stores?ships_to=<country>&capability=checkout` lists stores that
  ship there and accept UCP agent checkout, with each store's UCP MCP endpoint, payment handlers (add
  `payment=google.pay`), currency and, where published, return window (`min_return_days=30`).
- **Weekly dependency check:** `GET /v1/deps/digest?npm=<a,b>&pypi=<c>&apis=<stripe,github>&models=<gpt-4o>&mcp=<registry name>&x402=<host>&days=7`
  returns, in one call, what changed or needs a look across all of them (`attention` first). Run it on a schedule.
- **Before shopping on a site, free:** `GET /v1/verify/store?domain=<site>` is a signed short answer (agent checkout,
  endpoint, payment handlers, AI agents blocked).
- **Before building on a model:** `GET /v1/ai/retirements?model=<name>` shows when it shuts down on each platform
  (dates differ between platforms) and what replaces it. Without `model`, it lists the shutdowns coming up.

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
