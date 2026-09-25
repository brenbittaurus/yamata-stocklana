# Stock Strikes — Devnet

**Future-payoff positions as resolution-aware collateral. Active registry and market accounts checked 2026-09-25.**

Stock Strikes are Above/Below outcome contracts on an underlying equity reference. This demonstration is on **Solana Devnet**, denominated in **dUSDC test tokens**. It is not a claim of Mainnet derivative availability or production trading access.

## Programs and active series

- Stock Strikes program: [`zYmxwyYvT47LtUqD5dkY8v82mzxKUduVnnkecQnZwow`](https://explorer.solana.com/address/zYmxwyYvT47LtUqD5dkY8v82mzxKUduVnnkecQnZwow?cluster=devnet).
- Native credit-account program: [`7WxJUn3DiwfbU9PMAopXYUbRYKUzVTDnmZnVTUK7x1kL`](https://explorer.solana.com/address/7WxJUn3DiwfbU9PMAopXYUbRYKUzVTDnmZnVTUK7x1kL?cluster=devnet).
- Devnet quote mint (dUSDC, 6 decimals): [`DQ1RR3ToNseMNAFE5iE2CJv9cMW17VRBJXRPpPsb1eAE`](https://explorer.solana.com/address/DQ1RR3ToNseMNAFE5iE2CJv9cMW17VRBJXRPpPsb1eAE?cluster=devnet).
- Resolution configuration: [`AmmJGMruWHqqqrVYxHA9pcAN8CYFh3DqPCD2QE3i1SmL`](https://explorer.solana.com/address/AmmJGMruWHqqqrVYxHA9pcAN8CYFh3DqPCD2QE3i1SmL?cluster=devnet).

| Active series | Strike / condition | Market account |
|---|---|---|
| tsla-350-2026-10-09 | TSLA > $350 | [`Jjopvxxk7HZjpXXLo5pKVPuNrgzgQEHWjRqBXMeiy5U`](https://explorer.solana.com/address/Jjopvxxk7HZjpXXLo5pKVPuNrgzgQEHWjRqBXMeiy5U?cluster=devnet) |
| tsla-375-2026-10-09 | TSLA > $375 | [`3c6EebvaiezMokvJjswSJgXe8NEHjrHscFoDywPDBxdY`](https://explorer.solana.com/address/3c6EebvaiezMokvJjswSJgXe8NEHjrHscFoDywPDBxdY?cluster=devnet) |
| tsla-400-2026-10-09 | TSLA > $400 | [`5aNVKYE1SZXNunHeVRFxTnnqCrZSxGS8q7tffiDtNgu1`](https://explorer.solana.com/address/5aNVKYE1SZXNunHeVRFxTnnqCrZSxGS8q7tffiDtNgu1?cluster=devnet) |

The three registry entries are neither retired nor test markets. Fresh finalized account reads showed all three **open and unresolved**, with the listed mint/vault identities and times. “Active” is a registry/account-state observation, not a guarantee of an executable quote or sufficient liquidity at a later time.

| Series | Above mint | Below mint | dUSDC quote vault |
|---|---|---|---|
| TSLA $350 | [`4fhmLR26Y28kZnGAp8t1k7M7bUUc428TndmmV46HfBis`](https://explorer.solana.com/address/4fhmLR26Y28kZnGAp8t1k7M7bUUc428TndmmV46HfBis?cluster=devnet) | [`3WLtv652ezaYR9dgRqSvNfESWuuhpQtvanvY1FcqKFPp`](https://explorer.solana.com/address/3WLtv652ezaYR9dgRqSvNfESWuuhpQtvanvY1FcqKFPp?cluster=devnet) | [`4DxJSVNJ3Au3gHPY62Peby2ZNeRJs82Qk4Q3uHPDunf6`](https://explorer.solana.com/address/4DxJSVNJ3Au3gHPY62Peby2ZNeRJs82Qk4Q3uHPDunf6?cluster=devnet) |
| TSLA $375 | [`EdK8PRPJDvVWLjUTueEZ3tFETwUZK5xNqgD7BhK2pRpC`](https://explorer.solana.com/address/EdK8PRPJDvVWLjUTueEZ3tFETwUZK5xNqgD7BhK2pRpC?cluster=devnet) | [`2SMVSSMU8L1gY6G3TdsPVc8aVnELiXakXD2kPyM99JHr`](https://explorer.solana.com/address/2SMVSSMU8L1gY6G3TdsPVc8aVnELiXakXD2kPyM99JHr?cluster=devnet) | [`EWvXWvTvnF5TeysRWDuuEB5A9h5wZXujzvRbpApd6p7V`](https://explorer.solana.com/address/EWvXWvTvnF5TeysRWDuuEB5A9h5wZXujzvRbpApd6p7V?cluster=devnet) |
| TSLA $400 | [`DSmKeEiagZBHmtreZfNyHkZcmZDwoXcJiVepRTWpUkXc`](https://explorer.solana.com/address/DSmKeEiagZBHmtreZfNyHkZcmZDwoXcJiVepRTWpUkXc?cluster=devnet) | [`47RYV2PhRFDTHinn5iXvtNWHHWBp64du4vyWyfvy3WB3`](https://explorer.solana.com/address/47RYV2PhRFDTHinn5iXvtNWHHWBp64du4vyWyfvy3WB3?cluster=devnet) | [`EuHnwmZVpr2ncE6zvDgdtiDzec7vKDbFN2e7CDXsnEmD`](https://explorer.solana.com/address/EuHnwmZVpr2ncE6zvDgdtiDzec7vKDbFN2e7CDXsnEmD?cluster=devnet) |

## Expiry and settlement rule

All three active markets share:

| Field | Verified value |
|---|---|
| Close / expiry / settlement target | **2026-10-09 19:59:30 UTC** — 15:59:30 America/New_York (EDT) |
| Target Unix timestamp | **1791575970** |
| Accepted observation window | Target through target + **90 seconds** |
| Earliest configured resolution time | **2026-10-09 20:01:00 UTC**, timestamp **1791576060** |
| Underlying feed | **Equity.US.TSLA/USD** |
| Feed ID | `16dad506d7db8da01c87581c87ca897a012a153557d4d578c3b9c9e1bc0632f1` |
| Winning side | Above if price is **strictly greater** than the strike; equality resolves Below |
| Contract payout | **1.000000 dUSDC per winning share**; losing shares have no winning payout |

The settlement rule uses the first eligible Pyth update at or after the target within the 90-second window, with price and confidence checks. Selection is off-chain and resolution is authority-mediated; the on-chain program does not independently verify a Pyth proof or the globally first eligible observation.

The current registry requests a **600-second dispute window** and **no required bond**. Resolution eligibility is not guaranteed finalization time: proposal, dispute handling and finalization are separate operations. The historical test-market example below used its then-effective configuration and must not be used as evidence of the current dispute duration.

## Pyth reference versus house contract pricing

**Pyth supplies the underlying equity reference and settlement observation. House quotes price the Strike contracts.** Pyth's TSLA price is neither a $0–$1 contract probability quote nor an executable bid for an outcome share.

Before settlement, the native collateral adapter uses the house's exit bid, not purchase cost or maximum possible payout. Quoting, inventory/capacity and vault-solvency checks can prevent a position from being recognized or traded. A visible position is not necessarily collateral usable for more credit.

## Verified native collateral policy

| Setting | Inspected value |
|---|---:|
| Pre-resolution haircut on house bid value | **25%** |
| Nominal pre-resolution LTV after haircut | **50%** |
| Decay begins | **48 hours before settlement** |
| Linear decay reaches zero | **2 hours before settlement** |
| State freshness limit | **60 seconds** |
| Minimum usable house bid | **1% of payout** |
| Resolved winning-claim policy | **90% LTV, 0% haircut**, conditional on a valid, solvent vault; not automatic availability |
| Native mandate/Strike maturity buffer | **36 hours** in the recorded deployment readback |

Before decay, the illustrative contribution is:

`recognized shares × usable house bid × (1 − 25%) × 50%`

This is a collateral contribution, not debt creation and not the account's final available Buying Power. Debt, reservations, liquidity, other route gates and successful policy/account reconciliation still apply. Pre-resolution support is scoped to the Strike adapter, not arbitrary prediction assets.

For the active series:

- Decay begins **2026-10-07 19:59:30 UTC**.
- Pre-resolution collateral contribution reaches zero **2026-10-09 17:59:30 UTC**.
- With a 36-hour buffer against the configured resolution time, compatible native mandate expiry must be **strictly before 2026-10-08 08:01:00 UTC**. A default mandate is not automatically safe simply because the market has not expired.

## Executed buy-and-pledge example

[The active-series purchase in ONCHAIN_PROOF.md](ONCHAIN_PROOF.md#active-series-strike-buy-and-pledge) records 50 TSLA > $375 Above shares purchased for 20 dUSDC, deposited and pledged in one transaction. Its historical exit bid was 0.36 dUSDC/share, so the nominal pre-decay contribution was **50 × 0.36 × 0.75 × 0.50 = 6.75 dUSDC**. Buying/pledging those shares did not itself draw 6.75 dUSDC of debt. This is historical transaction evidence, not a statement that the shares remain held today.

## Historical resolution and redemption evidence

[ONCHAIN_PROOF.md](ONCHAIN_PROOF.md#historical-test-market-resolution-and-redemption) links a separate TSLA > $360 test market's proposal, finalization and redemption. A recorded Pyth observation at the 2026-09-18 target was **$364.475**, selecting Above. The winning test position redeemed 20 shares for 20 dUSDC.

The active $350 / $375 / $400 series has not been represented as settled or redeemed. The test-market evidence demonstrates one historical authority-mediated lifecycle, not a reliability guarantee or trustless settlement certification.

## Limits

House liquidity, resolution authority, off-chain observation selection, market configuration and native credit policy remain material dependencies. On-chain market ownership and successful historical transactions are verified; a source-to-bytecode build match and independent security audit are not claimed. See [SECURITY_AND_LIMITS.md](SECURITY_AND_LIMITS.md).
