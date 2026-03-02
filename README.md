# DFX Finance — Flash Loan Callback Reentrancy PoC

> **Educational Purpose Only** — This PoC is created for security research and education
> purposes only. It runs against a local simulation, not mainnet.

## Quick Start

```bash
git clone https://github.com/NomosLabs-Security/poc-dfx-finance-2022
cd poc-dfx-finance-2022
forge install foundry-rs/forge-std --no-git
forge test -vvvv
```

## Details

- **Exploit Date:** 2022-11-11
- **Loss:** ~$7.5M
- **Chain:** Ethereum
- **Expected Output:** Flash loan deposit reentrancy exploit, LP token theft
- **Full Analysis:** [NomosLabs Security Intelligence Archive](https://nomoslabs.io/archive/dfx-finance-2022)

## Description

DFX Finance's `flash()` function sent tokens to the borrower and called back
before checking repayment. The attacker deposited the flash-loaned tokens as
liquidity during the callback, minting LP tokens for free. The repayment check
(based on pool balance) passed because deposited tokens counted toward the
balance requirement.

### How It Works

1. **VulnerableDFXPool** simulates the DFX pool with flash loan + deposit reentrancy
2. **DFXAttacker** flash-borrows 90% of the pool, deposits it back as liquidity during the callback
3. The balance check passes (tokens are back in the pool), but the attacker now holds LP tokens
4. Attacker withdraws LP to extract real tokens — stealing from existing liquidity providers
5. **FixedDFXPool** demonstrates the fix: a shared `nonReentrant` modifier across `flash()`, `deposit()`, and `withdraw()`

## Key Contracts

- `src/Exploit.sol` — Vulnerable pool, fixed pool, and attacker contracts
- `test/Exploit.t.sol` — Foundry tests demonstrating the exploit and the fix

## Key Takeaway

This exploit demonstrated why flash loan functions must share a reentrancy guard
with all state-mutating functions (deposit, withdraw, swap). A balance-based
repayment check alone is insufficient when the borrower can manipulate pool
state during the callback.

## Disclaimer

Educational purposes only. This is a simplified simulation demonstrating the
vulnerability pattern, not a fork replay.

## License

MIT — For educational use only.
