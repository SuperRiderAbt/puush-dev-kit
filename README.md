# 🧰 Puush Developer Kit

Minimal ABIs and production addresses for third-party integrations built on
Puush on Cronos mainnet.

## 🌐 Network

| Item | Value |
| --- | --- |
| Chain | Cronos Mainnet |
| Chain ID | `25` |
| Public RPC | `https://evm.cronos.org` |

## 🚀 Production Contracts

### Launchers

| Version | Purpose | Address | ABI |
| --- | --- | --- | --- |
| V104 | Legacy untaxed launcher | `0x0F73F73707C0f365808b1C50a0187E17E7F897c9` | `Puush.min.json` |
| V106 | Taxed launcher | `0xE591af00B27Ef58C1F4C4A7a4Bb975c57a891633` | `PuushV106.min.json` |

## 📦 Included Files

| File | Purpose |
| --- | --- |
| `Puush.min.json` | Minimal V104 launcher ABI |
| `PuushV106.min.json` | Minimal V106 launcher ABI |
| `PuushFunds.readonly.min.json` | Minimal readonly funds ABI |
| `PuushFundsTaxedV106.readonly.min.json` | V106 taxed funds readonly ABI |
| `PuushTokenV106.readonly.min.json` | V106 token readonly ABI for live tax reads |

## 🧭 Basic Integration Flow

1. Watch `CoinCreated` on the relevant launcher.
2. Resolve the funds contract with `fundsMap(coinAddress)`.
3. Execute buys and sells through the launcher.
4. Use the funds contract for readonly reserves and quote helpers.
5. For V106 taxed tokens, read live tax config directly from the token.

Useful discovery reads:
- `fundsMap(coinAddress)`
- `coinCreators(coinAddress)`
- `coinStartTime(coinAddress)` on V106

## 💸 Trade Execution

Third-party integrations should route trading through the launcher:
- `buyCoins(coinAddress, minAmountToBuy)`
- `sellCoins(coinAddress, amountToSell, minEthGained)`

Do not call the funds contract's state-changing trade methods directly.
Both V104 and V106 funds contracts restrict buy/sell execution to the launcher.

Use the funds contract for readonly pricing and reserve data only.

## 🪙 Funds Contracts

Each launched coin has its own funds contract.

Use:
- `fundsMap(coinAddress)` on the launcher to discover it
- `PuushFunds.readonly.min.json` for legacy/general readonly usage
- `PuushFundsTaxedV106.readonly.min.json` for taxed V106 quote helpers

Useful V106 taxed quote helpers:
- `getNetAmountToBuy()` for net token-out after tax
- `getGrossReturn()` for gross sell return before deductions

Useful interpretation notes:
- V104 buy and sell execution is untaxed
- V106 `minAmountToBuy` should be treated as net tokens received after tax
- V106 `minEthGained` should be treated as net CRO received after deductions

## 🧾 V106 Tax Data

> The API is not the source of truth for mutable V106 tax configuration.
> External systems should treat the token contract as the live source of truth.

### Launch-time data from `CoinCreated`

- `taxed`
- `taxBps`
- `taxManager`
- `startTime`

### Live data from the token

Use `PuushTokenV106.readonly.min.json` and read:
- `taxManager()`
- `taxBps()`
- `tokenTaxBurnBps()`
- `taxRecipientCount()`
- `taxRecipients(index)`
- `taxRecipientSharesBps(index)`
- `fundsAddress()`
- `graduated()`

### Important V106 note

Launch-time event data is not enough for long-lived integrations:
- `taxBps` can change after launch
- `taxManager` can change after launch
- recipient wallets and shares can change after launch
- `tokenTaxBurnBps` is not included in `CoinCreated`
- trading can be gated by `startTime`

If you need current V106 tax configuration, refresh from the token directly or
follow the token events in `PuushTokenV106.readonly.min.json`.

## ⚠️ Notes

- These files expose a minimal public integration surface, not the full contract implementation.

## 🏷️ Version

`v1.0.6`
