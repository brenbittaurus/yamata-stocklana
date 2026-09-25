# Security and Limits

**Prototype verification material, not a security audit or production-readiness certification.**

## Controls observed in the inspected implementation

- **Account and authorization checks:** custody and lot/account identity, owner or authorized mandate, policy authorization, transaction signing and permitted execution paths.
- **Bounded execution:** route/program allowlists, mandate expiry and action/total limits, nonce/sequence checks, slippage/minimum-output protection and transaction simulation where used by the route.
- **Credit accounting:** debt and reservation accounting, attestation expiry, liquidity/capacity constraints and post-fill account reconciliation. Buying Power is not inferred merely from a success heading.
- **Valuation gates:** supported-asset checks, price/state freshness and confidence checks, haircuts, route-specific LTV and available liquidity. These controls are configuration- and route-dependent, not uniform guarantees across Mainnet and Devnet.
- **Strike-specific gates:** house-bid valuation, a 25% pre-resolution haircut, 50% nominal LTV with time decay, maturity compatibility, market/position identity and quote-vault solvency. Unsupported or unusable collateral is not credit capacity.
- **Ambiguous outcomes:** inspected native order handling distinguishes trade confirmation from Buying Power reconciliation and avoids treating an unresolved outcome as permission for a duplicate financial attempt.

The linked transactions demonstrate the specific successful flows described in this repository, not exhaustive testing of every failure path.

## Liquidation is not generalized

**Generalized Yamata liquidation is not yet implemented.** Freezing a position is not the same as selling collateral and repaying debt. The native prototype must not be treated as a complete liquidation or recovery system.

Kamino is a separate lender with its own collateral, interest and liquidation rules. Using Kamino in one Mainnet sample does not establish a unified Yamata liquidation engine or protection against loss.

## Material prototype risks

| Risk | Consequence / disclosure |
|---|---|
| Trusted policy attestation | Policy attestations are trusted components and remain subject to configured deployment, account and epoch caps. |
| Two Mainnet transactions | A successful borrow can survive a failed swap. The sample is not cross-transaction atomic. |
| External lending | Debt may accrue interest; collateral may be liquidated under the lender's rules. The exact 200 USDC principal receipt is not a claim that outstanding debt remains exactly 200 forever. |
| Tokenized-stock issuer and Token-2022 behavior | Transfer restrictions, issuer controls, scaled UI amounts, venue support and liquidity can affect balances and execution. Token amounts must be interpreted with mint decimals/extensions. |
| Oracle and RPC dependence | Stale, unavailable or inconsistent data can block valuation or reconciliation. This package makes no uptime claim. |
| Strike house pricing | Exit liquidity, spreads and capacity are house-dependent. An underlying equity price does not ensure the contract can be sold at a displayed mark. |
| Strike resolution authority | Settlement is authority-mediated and Pyth-referenced, not independently verified on-chain as the first eligible Pyth observation. |
| Upgradeable deployments | The verified Devnet programs are executable accounts owned by the upgradeable loader. This package does not establish immutability or independently audit upgrade governance. |
| Devnet fixtures | Mirrors, dUSDC, house liquidity and demonstration limits are not Mainnet economic assurance. Devnet state and availability can change. |

## Performance and coverage limits

The 4.7-second result is one successful, prepared-account, returning-wallet Mainnet sample. A separate unsuccessful attempt exists outside that sample. It is not a success-rate estimate, a latency distribution, a p95, a cold-onboarding benchmark, or a guarantee. No comparative workflow or speedup was measured by this sample. Finalized transaction status was checked later; finalization latency was not 4.7 seconds by inference.

The acquired Mainnet QQQx was not automatically collateralized. Native Devnet deposit/pledge evidence and a historical test-market redemption do not prove complete Mainnet collateral lifecycle coverage.

## Evidence and access

No independent security audit, production security certification or reproducible source-to-deployed-bytecode match is claimed. The live product is [stocklana.yamata.io](https://stocklana.yamata.io); viewing linked blockchain evidence requires no wallet connection or transaction.

Public transaction links reveal public participants and activity. This repository does not include private wallet labels, unrelated balances, signing material or operational configuration.

No investment, legal or regulatory advice is provided. This package is not an offer of securities, credit, trading access or guaranteed settlement.
