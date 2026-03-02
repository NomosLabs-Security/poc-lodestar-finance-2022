# Lodestar Finance Exploit PoC — plvGLP Oracle Manipulation

## Overview
- **Date:** 2022-12-10
- **Loss:** ~$6.5M
- **Chain:** Arbitrum
- **Category:** Oracle Manipulation
- **Block:** 45121904

## Vulnerability
Lodestar Finance's oracle read the plvGLP/GLP exchange rate from PlutusDAO's vault. By donating GLP directly to the vault, the attacker inflated the exchange rate without changing totalSupply, making plvGLP collateral appear overvalued.

## Reproduction
```bash
git clone https://github.com/NomosLabs-Security/poc-lodestar-finance-2022
cd poc-lodestar-finance-2022
forge install foundry-rs/forge-std --no-git
forge test -vvvv
```

## References
- [Lodestar Post-Mortem](https://blog.lodestarfinance.io/post-mortem-summary-13f5fe0bb336)
