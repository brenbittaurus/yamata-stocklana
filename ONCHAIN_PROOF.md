# On-chain Proof

**Mainnet external-liquidity orchestration and measured user flow; separate native-account and Stock Strike proofs on Devnet. Finalized RPC checks performed 2026-09-25.**

Solana Mainnet genesis: `5eykt4UsFv8P8NJdTREpY1vzqKqZKvdpKuc147dw2N9d`. Solana Devnet genesis: `EtWTRABZaYq6iMfeYKouRu166VU2xqa1wcaWoxPkrZBG`. Both Mainnet explorer pages were opened in a browser and displayed **Success / Finalized**. The linked historical Devnet transactions were also returned by finalized RPC with no transaction error.

## 1. Mainnet — external-liquidity orchestration: TSLAx collateral and Kamino credit

**Transaction:** [`3rUEferuZeTPT439RmsrE7s3PbFGywn1mkzvZbhXb3QdcEPJMh29eSG48Eyf55vWzj58JpKexL2Uqqx3VWyP2Q16`](https://explorer.solana.com/tx/3rUEferuZeTPT439RmsrE7s3PbFGywn1mkzvZbhXb3QdcEPJMh29eSG48Eyf55vWzj58JpKexL2Uqqx3VWyP2Q16?cluster=mainnet-beta)

- Slot: **449782073**. Block time: **2026-09-23 18:17:31 UTC**.
- Kamino Lending program: [`KLend2g3cP87fffoy8q1mQqGKjrxjC8boSyAYavgmjD`](https://explorer.solana.com/address/KLend2g3cP87fffoy8q1mQqGKjrxjC8boSyAYavgmjD?cluster=mainnet-beta).
- Invoked deposit-reserve-liquidity-and-obligation-collateral and borrow-obligation-liquidity instructions in the same successful transaction.
- Wallet TSLAx debit and Kamino reserve TSLAx receipt: **131656769 raw units = 1.31656769 TSLAx** (8 decimals).
- Wallet USDC receipt and Kamino USDC reserve debit: **200000000 raw units = exactly 200 USDC** (6 decimals).

This establishes an atomic TSLAx collateral deposit plus 200 USDC principal borrow inside this transaction. The saved obligation readback reports approximately 1.31656768 underlying TSLAx after collateral-receipt conversion/rounding; that is distinct from the exact 1.31656769 transferred. Subsequent debt readbacks include interest/accrual and must not be substituted for the exact principal receipt.

## 2. Mainnet — Raydium QQQx acquisition

**Transaction:** [`4pgrETruQM3Jn8SLuLi9uBpbtVXntBzPMpsFwCFLCgEBSR7KaxVcfXKCR3LxgRuk3gFzWMnMwqKcSXxYD2iVJtog`](https://explorer.solana.com/tx/4pgrETruQM3Jn8SLuLi9uBpbtVXntBzPMpsFwCFLCgEBSR7KaxVcfXKCR3LxgRuk3gFzWMnMwqKcSXxYD2iVJtog?cluster=mainnet-beta)

- Slot: **449782082**. Block time: **2026-09-23 18:17:33 UTC**.
- Raydium concentrated-liquidity program: [`CAMMCzo5YL8w4VFF8KVHrK22GGUsp5VTaW7grrKgrWqK`](https://explorer.solana.com/address/CAMMCzo5YL8w4VFF8KVHrK22GGUsp5VTaW7grrKgrWqK?cluster=mainnet-beta).
- Invoked **SwapV2**. Wallet USDC debit: **exactly 200 USDC**.
- QQQx wallet receipt: **26888667 raw units**, or **0.26888667 base tokens** at 8 decimals.
- The saved Token-2022 scaled-UI multiplier was **1.0034560758968376**, producing **0.26981596 QQQx displayed** at the sample. Raw token units and scaled display units are deliberately distinguished.

| Mainnet asset | Mint |
|---|---|
| TSLAx | [`XsDoVfqeBukxuZHWhdvWHBhgEHjGNst4MLodqsJHzoB`](https://explorer.solana.com/address/XsDoVfqeBukxuZHWhdvWHBhgEHjGNst4MLodqsJHzoB?cluster=mainnet-beta) |
| QQQx | [`Xs8S1uUs1zvS2p7iwtsG3b6fkhpvmwz4GYU3gWAmWHZ`](https://explorer.solana.com/address/Xs8S1uUs1zvS2p7iwtsG3b6fkhpvmwz4GYU3gWAmWHZ?cluster=mainnet-beta) |
| USDC | [`EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v`](https://explorer.solana.com/address/EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v?cluster=mainnet-beta) |

The same wallet token account received 200 USDC from Kamino before spending 200 USDC in the Raydium transaction. Its pre-existing USDC balance was preserved across the pair. This is a cash-flow attribution, not a claim that fungible USDC units carry distinct provenance labels.

### Funding attribution and evidence boundary

| Claim | Evidence basis |
|---|---|
| Kamino supplied the credit | Kamino borrow instruction, reserve debit and wallet +200 USDC receipt in transaction 1. |
| Raydium executed the acquisition | Raydium SwapV2 invocation, wallet −200 USDC and +26888667 raw QQQx in transaction 2. |
| Yamata LP did not fund this pair | Relevant Yamata program/account/escrow/LP accounts are absent from both transaction account sets; historical before/after LP token-balance snapshots are unchanged. |
| Native Yamata debt did not increase | Historical native account snapshots show unchanged debt; neither transaction invokes the native Yamata program. |
| QQQx became new collateral | **Not demonstrated and not performed by this route.** It was received in the wallet, not deposited or pledged. |

The unchanged LP/debt comparisons are supporting historical account snapshots, not standalone fields proved by the explorer page. No unrelated wallet or LP balances are reproduced here. These findings apply to this transaction pair, not all product activity.

## 3. Mainnet application benchmark — one successful sample

**Supported wording:** “4.7 seconds click-to-position-visible in one successful Mainnet sample, measured from Confirm to QQQx rendered on the benchmark page, with a returning/authenticated wallet and accounts prepared.”

| Milestone | Elapsed from Confirm |
|---|---:|
| Lending transaction constructed | 0.775 s |
| Kamino transaction confirmed | 1.411 s |
| TSLAx collateral and borrowed USDC verified | 2.690 s |
| Raydium transaction confirmed | 3.767 s |
| QQQx receipt verified | 4.477 s |
| External account reconciliation completed | 4.669 s |
| QQQx rendered on benchmark page | **4.700 s** |

The curated events and exact signature association are in [docs/benchmark-sample.json](docs/benchmark-sample.json).

- Sample began **2026-09-23 18:17:30.520 UTC**. Timings are recorded application wall-clock observations. They are not independently signed timing evidence or a guarantee of cross-clock precision.
- “Cold path” means collateral deposit and borrowing occurred after Confirm; it does **not** mean first login, a fresh wallet, empty-account creation inside the timer, or unprepared onboarding.
- Login/authentication, prior wallet setup and empty account preparation were excluded. A separate unsuccessful attempt is not included in this successful sample; no success-rate conclusion is supported.
- The rendering endpoint is the benchmark page, **not a native Yamata dashboard Buying Power update**.
- Confirmation milestones are not finalized-commitment latency. Both transactions were verified finalized later; do not retroactively label 4.700 s as finalization time.
- This is not an average, median, p95, sustained throughput, reliability guarantee or measured speedup against a manual workflow.

## 4. Devnet — native credit-account and Stock Strikes deployments

| Component | Verified executable program |
|---|---|
| Yamata native credit account | [`7WxJUn3DiwfbU9PMAopXYUbRYKUzVTDnmZnVTUK7x1kL`](https://explorer.solana.com/address/7WxJUn3DiwfbU9PMAopXYUbRYKUzVTDnmZnVTUK7x1kL?cluster=devnet) |
| Stock Strikes | [`zYmxwyYvT47LtUqD5dkY8v82mzxKUduVnnkecQnZwow`](https://explorer.solana.com/address/zYmxwyYvT47LtUqD5dkY8v82mzxKUduVnnkecQnZwow?cluster=devnet) |

Both were returned as executable accounts under Solana's upgradeable loader. The three active market accounts in [STOCK_STRIKES.md](STOCK_STRIKES.md) are owned by the listed Stock Strikes program. This verifies account identity, not a reproducible build or source-to-bytecode equivalence.

### Native stock-mirror credit trade

[`3mUWh4JPBEwtGgQ4zgxt8Nmfriy2XpmNoGnPt45nwUYuqWg8hkLRKV6Xhtr8D4MCjZ9jsPAAx9SuDQQ8RqpgCgqj`](https://explorer.solana.com/tx/3mUWh4JPBEwtGgQ4zgxt8Nmfriy2XpmNoGnPt45nwUYuqWg8hkLRKV6Xhtr8D4MCjZ9jsPAAx9SuDQQ8RqpgCgqj?cluster=devnet)

Finalized without error at slot **503965298**. Logs show native `reserve_and_execute` with **120000000 raw units (120 dUSDC)** drawn, a Raydium Devnet swap, and a native lot pledge in the same successful transaction. This is native Devnet credit evidence; it is not the 200 USDC Mainnet Kamino sample. It does not describe the wallet's current holdings or debt.

This is the native Devnet automatic acquisition/deposit/pledge proof, separate from the Mainnet wallet-only QQQx purchase. Refreshed Buying Power follows a separate post-fill policy re-attestation and account readback; it is not part of the swap transaction itself.

### Active-series Strike buy and pledge

[`s8xgAxVXHrhYauqKVbZbxs1GLCeqoZdAcuiRTp9AoSAxVFnti7EpjkHYDhqt5vGp4ToFfz4Q5jqPFFSxwhJAqDM`](https://explorer.solana.com/tx/s8xgAxVXHrhYauqKVbZbxs1GLCeqoZdAcuiRTp9AoSAxVFnti7EpjkHYDhqt5vGp4ToFfz4Q5jqPFFSxwhJAqDM?cluster=devnet)

Finalized without error at slot **503471041**. The TSLA > $375 active-series purchase acquired **50 Above shares for 20 dUSDC**. The recorded instruction sequence includes the Strike fill, collateral deposit and native pledge in one transaction; the on-chain logs include the successful pledge. The historical policy readback reported **6.75 dUSDC of collateral credit contribution**: 50 × 0.36 house bid × 0.75 after haircut × 0.50 nominal LTV. This is a contribution calculation, not a 6.75 dUSDC borrow and not a claim of a currently held position.

### Historical test-market resolution and redemption

This is a **separate TSLA > $360 test market**, not settlement of the active series:

- Market: [`7WHBLkYkLA6L3gr4owE4zu1fsTQnMPwP9LnVqur5GmJU`](https://explorer.solana.com/address/7WHBLkYkLA6L3gr4owE4zu1fsTQnMPwP9LnVqur5GmJU?cluster=devnet).
- Target observation: **2026-09-18 19:59:30 UTC**. Recorded Pyth TSLA price **36447500 × 10^-5 = $364.475**, with publish time exactly equal to the target. Above won.
- Proposal: [`5QjUiuN6n7amKReP4M4hZjA3ZVpXnTJZABxSr6PB5J3Fu1AqEgZWSnACLjMxxWkBWwPRmh2ZqaruDzkuyhKdAeqJ`](https://explorer.solana.com/tx/5QjUiuN6n7amKReP4M4hZjA3ZVpXnTJZABxSr6PB5J3Fu1AqEgZWSnACLjMxxWkBWwPRmh2ZqaruDzkuyhKdAeqJ?cluster=devnet) — finalized, slot **501405806**.
- Finalization: [`36Ldqgmmv1gN54drDTZX9edQ3TfVNtnFBuMsKoi8FEqiPjH5pcQAvzPtQQZgTUdC9Gd4ZQHkQwM3zFF3xQhLY2MG`](https://explorer.solana.com/tx/36Ldqgmmv1gN54drDTZX9edQ3TfVNtnFBuMsKoi8FEqiPjH5pcQAvzPtQQZgTUdC9Gd4ZQHkQwM3zFF3xQhLY2MG?cluster=devnet) — finalized, slot **501406208**.
- Redemption: [`4tVeqZrSkE5tEAzFYzUTvF7onjZSyA39a8vKYxzfDKzq1NScdKmqTxPu8sfLCpPeRdRj9r9auxhDbnG1LKRUN9Xg`](https://explorer.solana.com/tx/4tVeqZrSkE5tEAzFYzUTvF7onjZSyA39a8vKYxzfDKzq1NScdKmqTxPu8sfLCpPeRdRj9r9auxhDbnG1LKRUN9Xg?cluster=devnet) — finalized, slot **501406330**; recorded redemption of **20 winning shares for 20 dUSDC**.

The evidence commitment for the recorded settlement observation is SHA-256 `4f0c6c879a8593392fba1b68c2105a89fe7f1925c55e999b98b9566e0e95b048`. This illustrates the authority-mediated lifecycle and a successful redemption. It does not show an independent on-chain Pyth-proof verifier or completed settlement of the current active markets.

## Verification limitations

Explorer links are read-only and cluster-qualified. RPC records establish successful historical execution. Timing and policy/account snapshots are application evidence; they require the scope and caveats above. Current values, market status and program upgrades may change after the verification date.
