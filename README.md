<!--
parent:
  order: false
-->

<div align="center">
  <h1>EventFi Contracts Repository</h1>
</div>

<div align="center">
  <a href="https://github.com/Marcrus813/event-fi/releases/latest">
    <img alt="Version" src="https://img.shields.io/github/tag/Marcrus813/event-fi.svg" />
  </a>
  <a href="https://github.com/Marcrus813/event-fi/blob/main/LICENSE">
    <img alt="License: Apache-2.0" src="https://img.shields.io/github/license/Marcrus813/event-fi.svg" />
  </a>
</div>

EventFi Smart Contracts Project

## Installation

For prerequisites and detailed build instructions please read the [Installation](https://github.com/Marcrus813/event-fi/) instructions. Once the dependencies are installed, run:

```bash
git submodule update --init --recursive --remote
```
or
```bash
forge install foundry-rs/forge-std --no-commit
forge install OpenZeppelin/openzeppelin-contracts-upgradeable --no-commit
forge install OpenZeppelin/openzeppelin-contracts --no-commit
forge install OpenZeppelin/openzeppelin-foundry-upgrades --no-commit
```

Or check out the latest [release](https://github.com/Marcrus813/event-fi).

## Test And Deploy

```bash
$env:PRIVATE_KEY = "0x2a871"
$env:USDT_ADDRESS = "0x3C4249f1cDfaAAFf"
$env:OPENZEPPELIN_BASH_PATH = "C:/Users/65126/Documents/Git/bin/bash.exe"
```


### Test
```
forge test --ffi
```

### Deploy

```
forge script script/DeployerV2.s.sol:DeployerScript --rpc-url $RPC_URL --private-key $PRIVKEY --ffi

```

### Upgrade

```
forge script script/UpgradeInvestorSalePoolDeployerV2.s.sol:UpgradeInvestorSalePoolDeployer --rpc-url $RPC_URL --private-key $PRIVKEY --ffi
```

## Community


## Contributing

Looking for a good place to start contributing? Check out some [`good first issues`](https://github.com/Marcrus813/event-fi/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22).

For additional instructions, standards and style guides, please refer to the [Contributing](./CONTRIBUTING.md) document.
