# Architecture

**Three distinct proofs: Mainnet external-liquidity orchestration, the native Yamata account on Devnet, and resolution-aware Stock Strike collateral on Devnet.**

## Product boundary

The Yamata Universal Credit Account is the account abstraction presented to the user. Its intended interface is one Buying Power balance above the active credit and execution route. It must not add unsupported collateral, count the same collateral twice, or treat a newly acquired asset as spendable credit before recognition succeeds.

Existing liquidity underneath. One Yamata account above it.

This statement describes the account model, not proof that every venue or asset is integrated. The Mainnet external-credit sample and Devnet native-credit demonstration below have different custody, debt and risk boundaries.

## A. Mainnet — external-liquidity orchestration and measured user flow

```mermaid
flowchart LR
    U[Authenticated wallet: Confirm] --> K[Transaction 1: TSLAx deposit + USDC borrow in Kamino]
    K --> V[Verify collateral and borrowed USDC receipt]
    V --> R[Transaction 2: spend 200 USDC on Raydium]
    R --> Q[Verify QQQx wallet receipt]
    Q --> E[Reconcile external account]
    E --> UI[Render QQQx on benchmark page]
```

- Existing TSLAx moved from the user's wallet into Kamino collateral. Deposit and borrow were atomic **within transaction 1**.
- Kamino supplied exactly 200 USDC of borrowed principal. Raydium's Mainnet concentrated-liquidity pool executed the QQQx purchase in **transaction 2**.
- The two-transaction workflow is **not atomic end-to-end**. If the second transaction fails, the first can remain confirmed with an open Kamino loan; reconciliation is required rather than assuming rollback.
- QQQx remained in the wallet. This sample did not deposit or pledge the acquired QQQx into Kamino or the native Yamata credit account.
- External account reconciliation and benchmark-page rendering do not demonstrate a native Yamata dashboard Buying Power update.
- The Mainnet sample did not draw from a Yamata LP. It does not prove a Mainnet deployment of the Devnet native-credit flow.

The evidence in [ONCHAIN_PROOF.md](ONCHAIN_PROOF.md) establishes this specific route, not universal routing or best execution across all venues.

## B. Devnet — native Yamata account and automatic deposit/pledge

```mermaid
flowchart LR
    A[Recognized collateral + policy valuation] --> B[Attested Buying Power]
    B --> G[Owner or authorized mandate + policy gates]
    G --> T[Atomic trade: credit draw + swap + deposit + pledge]
    T --> C[Confirmed custody and debt changes]
    C --> P[Post-fill policy re-attestation]
    P --> U[Account readback: Buying Power current]
```

The demonstrated native stock flow combines a native credit reservation/draw, a Raydium Devnet swap, deposit of the guaranteed minimum received stock-mirror quantity, and a native pledge in one transaction. A fresh attestation may precede execution when required. The post-fill re-attestation is a separate transaction/state update; confirmed execution alone is not the same as reconciled Buying Power.

Where actual swap output exceeds the deposited minimum, the residual can remain in the wallet. Do not describe every acquired unit as pledged. The linked native trade demonstrates the deposit/pledge path, not an unconditional promise that every future purchase or route increases Buying Power.

Native credit is represented in the Yamata credit-account ledger and funded through its native liquidity path. This differs from the benchmark's external Kamino obligation. Devnet dUSDC and stock mirrors are test assets.

## C. Devnet — future-payoff positions as resolution-aware collateral

House bid/ask quotes price the Above/Below contracts. A supported buy can atomically execute the Strike fill, deposit the outcome position into Yamata collateral and pledge the lot. Policy subsequently values the pledged position using the applicable house exit bid, haircut, LTV, decay and solvency checks.

Pyth supplies the underlying TSLA reference and settlement observation; house quotes price the Strike contracts. Resolution is authority-mediated under the configured dispute process, not independently verified on-chain as the first eligible Pyth observation. See [STOCK_STRIKES.md](STOCK_STRIKES.md).

## Buying Power, collateral and debt

1. Eligibility means an asset **may** qualify under a policy; it does not mean credit has been drawn.
2. Successful custody/pledge, usable valuation, maturity compatibility and route capacity determine whether the asset contributes now.
3. Borrowing consumes capacity and creates debt. A purchased asset may add collateral contribution only after its own recognition succeeds; it does not cancel the debt used to buy it.
4. On the native route, account readback after re-attestation is authoritative for refreshed Buying Power. A displayed position, estimated contribution or confirmed fill is insufficient by itself.
5. A stale, blocked, unpledged or unsupported position must not be represented as available Buying Power. Debt and reservations cannot be ignored when presenting remaining capacity.

## Trust boundaries

The prototype depends on wallet authorization, policy signing/attestation, RPC availability, venue programs, oracle data, asset issuers and the Strike house/resolution authority. Some policy checks run off-chain, while account ownership, signatures, state transitions and transaction execution are enforced on-chain. Neither set replaces the other. Source inspection is not a verifiable-build match to deployed bytecode; no such match is claimed by this package.
