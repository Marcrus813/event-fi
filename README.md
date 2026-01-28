# EventFi Contracts

EventFi is an innovative Web3 project that combines offline merchant activities with blockchain economics through NFT, staking, and event airdrop mechanisms, eliminating intermediaries on the platform.

For example, when a community helps a project launch an event, the project needs to transfer funds to the community, which then distributes them to users through hackathon rewards or other activities. In this scenario, the community acts as an intermediary. We need to eliminate this intermediary - projects can directly deposit event tokens into the contract, so tokens no longer pass through the community team's hands.

## Project Overview

The EventFi ecosystem includes the following core features:

- **Token Sale**: Users can purchase FCC tokens with USDT
- **NFT System**: Supports Basic NFT (8 USDT) and Pro NFT (80 USDT) for mining and event rewards
- **Staking**: Users can stake FCC to earn APR rewards, with Booster NFTs providing additional bonuses
- **Event Airdrops**: Merchants can launch airdrop events where participants receive FCC rewards
- **Redemption Mechanism**: After project completion, users can redeem USDT proportionally with FCC

## Business Process Architecture

### FCC Token Distribution

Total supply: **1 billion FCC**, distributed as follows:

1. **Sale Portion (10%)**:
   - **DirectSaleManager (20%)**: Direct sales, 100% USDT goes to redemption pool (This portion is cancelled, the 20% is reallocated to Staking and Foundation)
   - **InvestorSaleManager (10%)**: Investor sales, 50% USDT goes to redemption pool, 50% remains in contract

2. **NFT Manager (20%)**: Users receive FCC rebates when purchasing NFTs, 75% of USDT from sales goes to redemption pool
   - Basic NFT: 100 FCC rebate
   - Pro NFT: 1,000 FCC rebate

3. **EventManager (30%)**: Event airdrop mining pool, merchants launching events can receive mining rewards

4. **Foundation (20%)**: Foundation allocation

5. **Ecosystem (10%)**: Ecosystem allocation

6. **Staking (10%)**: Staking reward pool

### Core Contract Interactions

```
User ─► DirectSaleManager ──► Redemption Pool (100% USDT)
      │                       │
      └► InvestorSaleManager ─┤ (50% USDT)
                              │
User ─► NftManager ───────────┤ (75% USDT)
      │                       │
      ├► Basic NFT (8 USDT)   │
      └► Pro NFT (80 USDT)    │
      │                       ▼
      └─► Receive 100/1000 FCC  Redemption Pool
                              ▲
User ─► EventManager          │
      └► Airdrop Events/Mining │
                              │
User ─► Staking ──────────────┘
      └► Staking + Booster NFT
```

## Core Contract Details

### 1. DirectSalePool (Direct Sale Pool)

- **Price**: 1 USDT = 10 FCC
- **USDT Flow**: 100% goes to redemption pool
- **Purpose**: Direct purchase channel for regular users

### 2. InvestorSalePool (Investor Sale Pool)

- **Price**: Dynamically calculated based on current pool state
- **USDT Flow**: 50% goes to redemption pool, 50% remains in contract
- **Purpose**: Purchase channel for investors

### 3. NftManager (NFT Manager)

Manages two types of NFTs:

#### Membership NFTs

- **Basic NFT**: 8 USDT, 100 FCC rebate, 30-day validity
- **Pro NFT**: 80 USDT, 1,000 FCC rebate, 30-day validity
- **USDT Flow**: 75% to redemption pool, 20% to NFT Manager, 5% for other purposes

#### Booster NFT (Staking Bonus NFT)

One-time use NFTs that provide additional APR bonuses when staking:

- **Legendary Tuna**: >= 1600 FCC, 20% APR bonus
- **Epic Salmon**: >= 1000 FCC, 15% APR bonus
- **Rare Shrimp**: >= 160 FCC, 10% APR bonus
- **Uncommon EventFi**: >= 100 FCC, 5% APR bonus

### 4. EventManager (Event Manager)

- **Function**: Merchants can launch airdrop events, users participate to claim rewards
- **Mining Mechanism**: Merchants receive mining rewards after events end (requires holding NFT)
- **Reward Pool**: 30% of total FCC supply for event mining
- **Mining Decay**:
  - < 30 million: 50% rewards
  - < 100 million: 40% rewards
  - < 200 million: 20% rewards
  - < 300 million: 10% rewards

### 5. StakingManager (Staking Manager)

Users stake FCC to earn fixed APR rewards:

| Lock Period | Base APR | Actual Yield (with Booster) |
| ----------- | -------- | --------------------------- |
| 30 days     | 3%       | Actual yield 0.25%          |
| 60 days     | 6%       | Actual yield 0.99%          |
| 90 days     | 9%       | Actual yield 2.22%          |
| 180 days    | 15%      | Actual yield 7.40%          |

- **Booster NFT Bonus**: Users can bind Booster NFTs when staking for an additional 5%-20% APR
- **APR Halving**: All APRs are halved after October 31, 2025
- **Minimum Stake**: 10 FCC
- **Reward Pool**: 10% of total FCC supply for staking rewards

### 6. RedemptionPool (Redemption Pool)

- **Lock Period**: 1095 days (approximately 3 years)
- **Redemption Mechanism**: Users burn FCC to receive proportional USDT from the pool
- **Calculation Formula**: Redeemed USDT = (Total USDT in pool × FCC burned) / Total FCC supply

## Quick Start

### Install Dependencies

```bash
# Install Foundry (if not already installed)
curl -L https://foundry.paradigm.xyz | bash
foundryup

# Install project dependencies
forge install
```

### Compile Contracts

```bash
forge build
```

### Run Tests

```bash
# Run all tests
forge test

# Run specific test with verbose output
forge test --match-path test/YourTest.t.sol -vvv

# Generate gas report
forge test --gas-report
```

### Code Formatting

```bash
forge fmt
```

### Local Development

```bash
# Start local node
anvil

# Deploy contracts (in another terminal)
forge script script/Deploy.s.sol --rpc-url http://localhost:8545 --private-key <your_private_key> --broadcast
```

## Project Structure

```
EventFi-contracts/
├── src/
│   └── contracts/
│       ├── core/
│       │   ├── EventFiEventManager.sol    # Event management
│       │   ├── StakingManager.sol          # Staking management
│       │   ├── RedemptionPool.sol          # Redemption pool
│       │   ├── sale/
│       │   │   ├── DirectSalePool.sol      # Direct sales
│       │   │   └── InvestorSalePool.sol    # Investor sales
│       │   └── token/
│       │       ├── EventFiCoin.sol        # FCC token
│       │       └── NftManagerV5.sol        # NFT management
│       └── interfaces/                     # Contract interfaces
├── test/                                   # Test files
├── lib/                                    # Dependencies
├── foundry.toml                            # Foundry configuration
└── remappings.txt                          # Import path mappings
```
