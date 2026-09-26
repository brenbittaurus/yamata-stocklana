# Stock Strikes — Devnet

**Future-payoff positions as resolution-aware collateral. Active registry, program bytecode and market accounts checked 2026-09-26.**

Stock Strikes are Above/Below outcome contracts on an underlying equity reference. This demonstration is on **Solana Devnet**, denominated in **dUSDC test tokens**. It is not a claim of Mainnet derivative availability or production trading access.

## Programs and active series

Two Stock Strikes programs coexist on Devnet. Every market belongs to exactly one of them and settles there.

- **Hardened companion program (active series):** [`FP1A5Lk4ZZ8gcxC6c2qx3aeyjZAhcc7JrKVqrJNvnMhg`](https://explorer.solana.com/address/FP1A5Lk4ZZ8gcxC6c2qx3aeyjZAhcc7JrKVqrJNvnMhg?cluster=devnet). Deployed 2026-09-26 from the reviewed source; on-chain ProgramData sha256 `033de1be4219dfae366859eb62c5c462fa317190bca008c225c8b879f63d4182` (453,904 bytes) matches the built artifact byte-for-byte. Deploy transaction [`2WidGs6v…`](https://explorer.solana.com/tx/2WidGs6vbkpZhrmESaYkeLHnzwv4jqsrcrpmgRAPCkHxqpCHCE2KMexX34b39igWLi32vveqUmsuqqJDdsLmH3Tx?cluster=devnet). Protocol config [`52iCN14LnNSK76EgVFoqnW4GRBayZ357NFEkzR7BJyHB`](https://explorer.solana.com/address/52iCN14LnNSK76EgVFoqnW4GRBayZ357NFEkzR7BJyHB?cluster=devnet), resolution configuration [`BwCiiGzRbrotKjLPEiAp33ZFw9jqAiKk3w9W1o6sw4nN`](https://explorer.solana.com/address/BwCiiGzRbrotKjLPEiAp33ZFw9jqAiKk3w9W1o6sw4nN?cluster=devnet) (600-second dispute window, no bond). Same admin, settlement/oracle authority and dUSDC mint as the legacy program.
- **Legacy program (retired series):** [`zYmxwyYvT47LtUqD5dkY8v82mzxKUduVnnkecQnZwow`](https://explorer.solana.com/address/zYmxwyYvT47LtUqD5dkY8v82mzxKUduVnnkecQnZwow?cluster=devnet). Its bytecode was not changed; the October 2 and October 9 series remain on it for existing positions, exits, redemption and settlement.
- Native credit-account program: [`7WxJUn3DiwfbU9PMAopXYUbRYKUzVTDnmZnVTUK7x1kL`](https://explorer.solana.com/address/7WxJUn3DiwfbU9PMAopXYUbRYKUzVTDnmZnVTUK7x1kL?cluster=devnet).
- Devnet quote mint (dUSDC, 6 decimals): [`DQ1RR3ToNseMNAFE5iE2CJv9cMW17VRBJXRPpPsb1eAE`](https://explorer.solana.com/address/DQ1RR3ToNseMNAFE5iE2CJv9cMW17VRBJXRPpPsb1eAE?cluster=devnet).

| Active series | Strike / condition | Market account (companion program) |
|---|---|---|
| tsla-350-2026-10-16 | TSLA > $350 | [`A6PXrmy5BmWgVZ5SAeKnDjTiy4sHez1tBuDKr3bBRLkt`](https://explorer.solana.com/address/A6PXrmy5BmWgVZ5SAeKnDjTiy4sHez1tBuDKr3bBRLkt?cluster=devnet) |
| tsla-375-2026-10-16 | TSLA > $375 | [`Bjnr7q6SsLVMc3t5RwZWrUMdPtCGYtAeBfs4hA2K9uxX`](https://explorer.solana.com/address/Bjnr7q6SsLVMc3t5RwZWrUMdPtCGYtAeBfs4hA2K9uxX?cluster=devnet) |
| tsla-400-2026-10-16 | TSLA > $400 | [`FnLbZLKTfy9FYwjyVxwFbB56UeuxKGRA8LXG7sq1AUgG`](https://explorer.solana.com/address/FnLbZLKTfy9FYwjyVxwFbB56UeuxKGRA8LXG7sq1AUgG?cluster=devnet) |

The three registry entries are neither retired nor test markets. Finalized account reads on 2026-09-26 showed all three **open and unresolved**, owned by the companion program, with the listed vault identities and times, and each vault pre-funded with 2,000 dUSDC. “Active” is a registry/account-state observation, not a guarantee of an executable quote or sufficient liquidity at a later time.

## Expiry and settlement rule

All three active markets share:

| Field | Verified value |
|---|---|
| Close / expiry / settlement target | **2026-10-16 19:59:30 UTC** — 15:59:30 America/New_York (EDT) |
| Target Unix timestamp | **1792180770** |
| Accepted observation window | Target through target + **90 seconds** |
| Earliest configured resolution time | **2026-10-16 20:01:00 UTC**, timestamp **1792180860** |
| Underlying feed | **Equity.US.TSLA/USD** |
| Feed ID | `16dad506d7db8da01c87581c87ca897a012a153557d4d578c3b9c9e1bc0632f1` |
| Winning side | Above if price is **strictly greater** than the strike; equality resolves Below |
| Contract payout | **1.000000 dUSDC per winning share**; losing shares have no winning payout |

The settlement rule uses the first eligible Pyth update at or after the target within the 90-second window, with price and confidence checks. Selection is off-chain and resolution is authority-mediated; the on-chain program does not independently verify a Pyth proof or the globally first eligible observation.

The companion resolution configuration requests a **600-second dispute window** and **no required bond**. Resolution eligibility is not guaranteed finalization time: proposal, dispute handling and finalization are separate operations run by the Devnet settlement keeper. The historical test-market example below used its then-effective configuration and must not be used as evidence of the current dispute duration.

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

- Decay begins **2026-10-14 19:59:30 UTC**.
- Pre-resolution collateral contribution reaches zero **2026-10-16 17:59:30 UTC**.
- With a 36-hour buffer against the configured resolution time, compatible native mandate expiry must be **strictly before 2026-10-15 08:01:00 UTC**; a fresh seven-day onboarding mandate therefore remains eligible for this series until **2026-10-08 08:01:00 UTC**. A default mandate is not automatically safe simply because the market has not expired.

## Executed buy, pledge, repayment and sale on the active series

On 2026-09-26, while the US equity market was closed, a brand-new Devnet wallet activated a demo account, bought 50 TSLA > $375 Above shares for 20 dUSDC from cash and pledged them in one transaction ([`5QtHNWia…`](https://explorer.solana.com/tx/5QtHNWiaENLDEAJBu1eDfFf2QdSYNKgVwD5ukAyZUBbEBKqJibi4oqP44zj91sA7WEPVbrcBv9vxposDNp1u2dBJ?cluster=devnet)). At the 0.36 dUSDC/share exit bid the position contributed **50 × 0.36 × 0.75 × 0.50 = 6.75 dUSDC** of pre-decay collateral value. The same account then repaid its 200 dUSDC stock-purchase debt in full ([`22or5Eyi…`](https://explorer.solana.com/tx/22or5EyiVgJNv4kDR3bSXaYXtYXfT7f9zydr4wmHPhhdShDBq4EyFhAjgP8BfFJKUXofVXbnxWtd1fk9kBdo9Zq8?cluster=devnet)) and sold all 50 pledged shares back to the house for 18 dUSDC after the zero-debt release gate cleared ([`3Z3qtkDQ…`](https://explorer.solana.com/tx/3Z3qtkDQdhAgNWJMJAWtVtnnUgMTSufMCK8S2dAbEcJfgimZixzbDFNHV11Lihjw9UZJpugAvZwiYVyzxhDEWp5n?cluster=devnet)). A separate account exercised a credit-funded repeat purchase in the same market after authorizing the companion venue on its mandate (300 further shares, 80 dUSDC cash + 40 dUSDC borrowed), then sold all 350 shares in one multi-lot exit. Buying/pledging never draws the collateral contribution as debt. These are historical transactions, not statements that the shares remain held today.

## Historical resolution and redemption evidence

[ONCHAIN_PROOF.md](ONCHAIN_PROOF.md#historical-test-market-resolution-and-redemption) links a separate TSLA > $360 test market's proposal, finalization and redemption. A recorded Pyth observation at the 2026-09-18 target was **$364.475**, selecting Above. The winning test position redeemed 20 shares for 20 dUSDC.

The active $350 / $375 / $400 series has not been represented as settled or redeemed. The test-market evidence demonstrates one historical authority-mediated lifecycle, not a reliability guarantee or trustless settlement certification.

## Limits

House liquidity, resolution authority, off-chain observation selection, market configuration and native credit policy remain material dependencies. On-chain market ownership, the companion program's source-to-bytecode match and successful historical transactions are verified; an independent security audit is not claimed. See [SECURITY_AND_LIMITS.md](SECURITY_AND_LIMITS.md).
