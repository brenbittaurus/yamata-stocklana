# Stocklana by Yamata

**One portfolio. One Buying Power balance. Every eligible position.**

Stocklana is a Solana implementation of Yamata's **Universal Credit Account** — an account layer that turns eligible on-chain financial positions into risk-adjusted Buying Power that can be reused across integrated markets.

Instead of forcing the user to manually move between a wallet, lending venue, execution venue and portfolio tools, Yamata coordinates the underlying markets and presents them as one account.

> **Existing liquidity underneath. One Yamata account above it.**

---

## Submission

| Resource | Link |
|---|---|
| **Live App** | [stocklana.yamata.io](https://stocklana.yamata.io) |
| **Pitch Deck** | [View the deck](https://docsend.com/view/uvk6skupzjn8dpu3) |
| **Demo / Technical Walkthrough** | [Watch the walkthrough](https://youtu.be/WdUl8vnYkAQ) |

## The problem

Tokenized stocks are already on-chain and composable.

But the user experience is still fragmented.

A user may need to:

1. hold an asset in a wallet;
2. move it into a lending venue;
3. borrow against it;
4. move the borrowed liquidity;
5. execute the next trade somewhere else;
6. keep track of collateral, debt and positions across multiple products.

The protocols may be composable.

The **account experience is not**.

Stocklana explores what happens when those separate markets are made to feel like one brokerage-style account.

## The Yamata model

On Yamata, eligible positions contribute to a single risk-adjusted Buying Power balance.

The product model is:

```text
PORTFOLIO
   |
   v
YAMATA UNIVERSAL CREDIT ACCOUNT
   |
   v
BUYING POWER
   |
   +--> BUY
   +--> COPY
   +--> TRADE
```

Buying Power reflects recognized collateral, risk treatment, outstanding debt, reservations and available route capacity. The user sees the account's remaining usable credit; Yamata coordinates the underlying collateral, credit and execution steps.

## From an existing position to the next trade

Stocklana brings the account model to tokenized public stocks and structured positions on Solana.

- **Use existing positions.** Eligible stock collateral can support the next purchase.
- **Put Buying Power to work.** Buy, copy a specific trade, or trade through the account experience.
- **Recognize newly acquired collateral.** The native Devnet flow combines credit draw, swap, deposit and pledge, then refreshes Buying Power through post-fill reconciliation.
- **Include future-payoff positions.** Eligible unresolved Stock Strikes contribute conservative, resolution-aware collateral value alongside stock positions on Devnet.

### Demonstrated flows

| Network | Flow | Evidence |
|---|---|---|
| **Mainnet** | Real TSLAx collateral → Kamino credit → Raydium QQQx purchase | [Collateral + borrow](https://explorer.solana.com/tx/3rUEferuZeTPT439RmsrE7s3PbFGywn1mkzvZbhXb3QdcEPJMh29eSG48Eyf55vWzj58JpKexL2Uqqx3VWyP2Q16?cluster=mainnet-beta) · [QQQx purchase](https://explorer.solana.com/tx/4pgrETruQM3Jn8SLuLi9uBpbtVXntBzPMpsFwCFLCgEBSR7KaxVcfXKCR3LxgRuk3gFzWMnMwqKcSXxYD2iVJtog?cluster=mainnet-beta) |
| **Devnet** | Native credit draw → swap → automatic deposit → pledge → refreshed Buying Power | [Trade transaction](https://explorer.solana.com/tx/3mUWh4JPBEwtGgQ4zgxt8Nmfriy2XpmNoGnPt45nwUYuqWg8hkLRKV6Xhtr8D4MCjZ9jsPAAx9SuDQQ8RqpgCgqj?cluster=devnet) · [Account flow](ARCHITECTURE.md#b-devnet--native-yamata-account-and-automatic-depositpledge) |
| **Devnet** | Stock Strike purchase → deposit → pledge → resolution-aware contribution | [Buy and pledge](https://explorer.solana.com/tx/s8xgAxVXHrhYauqKVbZbxs1GLCeqoZdAcuiRTp9AoSAxVFnti7EpjkHYDhqt5vGp4ToFfz4Q5jqPFFSxwhJAqDM?cluster=devnet) · [Strike policy](STOCK_STRIKES.md#verified-native-collateral-policy) |

### Mainnet measurement: 4.7 seconds

In the recorded Mainnet sample, **4.7 seconds elapsed from Confirm to QQQx appearing on the benchmark page**. The flow deposited TSLAx as collateral, borrowed **200 USDC from Kamino**, purchased QQQx on **Raydium**, and reconciled the external account.

[View the transaction timeline](ONCHAIN_PROOF.md#3-mainnet-application-benchmark--one-successful-sample) · [Inspect the timing data](docs/benchmark-sample.json)

## Stock Strikes

Stock Strikes are **Above / Below contracts on an equity reference**. They extend the account model beyond spot holdings to eligible positions with a future payoff.

In the Devnet demonstration, unresolved Strike positions are valued from the house exit bid, with a haircut and collateral factor that declines as settlement approaches. **Pyth supplies the underlying equity reference and settlement observation; house quotes price the Strike contracts.**

The recorded TSLA > $375 example bought and pledged **50 Above shares for 20 dUSDC**. At the recorded house bid and pre-decay policy, those shares contributed **6.75 dUSDC** toward the native credit ceiling.

[Explore Stock Strikes](STOCK_STRIKES.md) · [View the historical resolution and redemption](ONCHAIN_PROOF.md#historical-test-market-resolution-and-redemption)

## Built on Solana markets

| Component | Role in the submission |
|---|---|
| **Yamata Universal Credit Account** | Coordinates collateral recognition, credit policy, execution and account reconciliation. |
| **Solana** | On-chain accounts, token custody and transaction execution. |
| **Kamino** | External collateral and USDC credit in the Mainnet flow. |
| **Raydium** | Tokenized-stock swap execution. |
| **Pyth** | Underlying equity reference prices and Stock Strike settlement observations. |
| **Stock Strikes** | Resolution-aware structured positions in the Devnet portfolio. |

## Explore the submission

| Guide | What you'll find |
|---|---|
| [Architecture](ARCHITECTURE.md) | Account flows, collateral recognition, credit and post-fill reconciliation. |
| [On-chain proof](ONCHAIN_PROOF.md) | Transactions, token movements, deployed programs and the measured Mainnet timeline. |
| [Stock Strikes](STOCK_STRIKES.md) | Markets, pricing, collateral policy and settlement evidence. |
| [Security and limits](SECURITY_AND_LIMITS.md) | Trust boundaries, risk controls and current technical limitations. |
| [Benchmark data](docs/benchmark-sample.json) | The recorded application milestones behind the 4.7-second result. |

---

**One portfolio. One Buying Power balance. Every eligible position.**

[Open Stocklana](https://stocklana.yamata.io) · [Watch the walkthrough](https://youtu.be/WdUl8vnYkAQ) · [View the pitch deck](https://docsend.com/view/uvk6skupzjn8dpu3)
