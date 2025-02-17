# DeFi Bridge: Bitcoin-Stablecoin Liquidity Protocol

A sophisticated decentralized finance protocol enabling seamless Bitcoin collateralization, stablecoin minting, and automated market making.

## Overview

The DeFi Bridge protocol provides a secure and efficient way to:

- Mint stablecoins using Bitcoin as collateral
- Provide liquidity to earn trading fees
- Participate in automated market making
- Manage collateralized debt positions

## Key Features

- **Over-collateralized Lending**

  - 150% minimum collateral ratio
  - Dynamic liquidation threshold at 130%
  - Secure vault management

- **Automated Market Making**

  - Constant product AMM model
  - Dynamic pricing based on pool ratios
  - Efficient liquidity provision

- **Price Oracle Integration**
  - Secure price feed implementation
  - Price validation and safety checks
  - Maximum price boundaries

## Protocol Parameters

### Collateral Requirements

- Minimum Collateral Ratio: 150%
- Liquidation Threshold: 130%
- Minimum Deposit: 0.01 BTC (1,000,000 sats)

### Pool Parameters

- Pool Fee Rate: 0.3%
- Price Precision: 6 decimal places
- Maximum Price: 1M USD (with 6 decimal precision)
- Maximum Mint Amount: 10K USD (with 6 decimal precision)

## Core Functions

### Vault Management

```clarity
(define-public (deposit-collateral (btc-amount uint)))
```

Deposit Bitcoin as collateral into a vault.

- Requires minimum deposit of 0.01 BTC
- Updates vault collateral balance
- Tracks deposit timestamps

### Stablecoin Operations

```clarity
(define-public (mint-stablecoin (amount uint)))
```

Mint stablecoins against deposited collateral.

- Validates collateral ratio requirements
- Ensures sufficient backing
- Updates total supply

```clarity
(define-public (burn-stablecoin (amount uint)))
```

Burn stablecoins to reduce debt position.

- Reduces vault debt
- Updates total supply
- No withdrawal fees

### Liquidity Pool Operations

```clarity
(define-public (add-liquidity (btc-amount uint) (stable-amount uint)))
```

Provide liquidity to the AMM pool.

- Calculates and mints LP tokens
- Updates pool balances
- Tracks provider contributions

```clarity
(define-public (remove-liquidity (lp-tokens uint)))
```

Remove liquidity from the pool.

- Burns LP tokens
- Returns proportional assets
- Updates pool state

### Read-Only Functions

```clarity
(define-read-only (get-vault-details (owner principal)))
```

Retrieve vault information for an address.

```clarity
(define-read-only (get-collateral-ratio (owner principal)))
```

Calculate current collateral ratio for a vault.

```clarity
(define-read-only (get-pool-details))
```

Get current pool statistics and state.

```clarity
(define-read-only (get-lp-details (provider principal)))
```

Retrieve liquidity provider information.

## Error Codes

| Code | Description             |
| ---- | ----------------------- |
| 1000 | Not authorized          |
| 1001 | Insufficient balance    |
| 1002 | Invalid amount          |
| 1003 | Insufficient collateral |
| 1004 | Pool empty              |
| 1005 | Slippage too high       |
| 1006 | Below minimum           |
| 1007 | Above maximum           |
| 1008 | Already initialized     |
| 1009 | Not initialized         |
| 1010 | Invalid price           |

## Security Features

1. **Access Control**

   - Owner-only functions for critical operations
   - Validated initialization process
   - Protected price updates

2. **Safety Checks**

   - Minimum deposit requirements
   - Maximum mint limits
   - Price validation
   - Collateral ratio enforcement

3. **Balance Protection**
   - Safe arithmetic operations
   - Balance verification before transfers
   - Pool ratio maintenance

## Protocol Math

### Collateral Ratio Calculation

```clarity
(* btc-value-usd 100) / stablecoin-amount
```

- Ensures 150% minimum collateral ratio
- Uses 6 decimal precision for accuracy

### LP Token Calculation

```clarity
Initial: sqrt(btc_amount * stable_amount)
Subsequent: (btc_amount * sqrt(pool_btc * pool_stable)) / pool_btc
```

- Fair value calculation
- Prevents manipulation
- Maintains pool ratios

## Best Practices

1. **Vault Management**

   - Maintain healthy collateral ratio above 150%
   - Monitor price fluctuations
   - Repay debt before removing collateral

2. **Liquidity Provision**

   - Provide balanced amounts
   - Consider impermanent loss
   - Monitor pool fees earned

3. **Risk Management**
   - Watch liquidation threshold
   - Maintain buffer above minimum ratio
   - Consider market volatility
