# Pallet dapps-staking RPC API
This document describes the interface for the pallet-dapps-staking.

Table of Contents:
1. [Terminology](#Terminology)
2. [Referent implementatio](#Referent)
3. [FAQ](#FAQ)

## Terminology
### Actors in dApps Staking

- `developer`: a developer or organization who deploys the smart contract
- `staker`: any Astar user who stakes tokens on the developer's smart contract


### Abbreviations and Terminology
- `dApp`: decentralized application, is an application that runs on a distributed network.
- `smart contract`: on-chain part of the dApp
- `contract`: short for smart contract
- `EVM`: Ethereum Virtual Machine. Solidity Smart contract runs on it.
- `ink!`: Smart Contract written in Rust, compiled to WASM.
- `era`: Period of time. After it ends, rewards can be claimed. It is defined by the number of produced blocks. Duration of an era for this pallet is around 1 day. The exact duration depends on block production duration.
- `claim`: Claim ownership of the rewards from the contract's reward pool.
- `bond`: Freeze funds to gain rewards.
- `stake`: In this pallet a staker stakes bonded funds on a smart contract .
- `unstake`: Unfreeze bonded funds and stop gaining rewards.
- `wasm`: Web Assembly.
- `contracts's reward pool`: Sum of unclaimed rewards on the contract. Including developer and staker parts.

---


---
## Referent API implementation
https://github.com/AstarNetwork/astar-apps

---
## FAQ

### When do the projects/developers get their rewards?
The earned rewards need to be claimed by calling claim() function. Once the claim() function is called all stakers on the contract and the developer of the contract get their rewards. This function can be called from any account. Recommended is that it is called by the projects/developers on a daily or at most weekly basis.

### What happens if nobody calls the claim function for longer than 'history_depth' days?
The un-claimed rewards older than 'history_depth' days will be burnt.

### When developers register their dApp, which has no contract yet, what kind of address do they need to input?
There has to be a contract. Registration can’t be done without the contract.

### Can projects/developers change contract address once it is registered for dApps staking?
The contract address can't be changed for the dApps staking. However, if the project needs to deploy new version of the contract, they can still use old (registered) contract address for dApp staking purposes.

### How do projects/developers (who joins dApps staking) get their stakers' address and the amount staked?
```
ContractEraStake(contract_id, era).stakers
```
This will give the vector of all staker' accounts and how much they have staked.

### What is the maximum numbers of stakers per dapps?
Please check in the source code constant `MaxNumberOfStakersPerContract`.

### What is the minimum numbers of stakers per dapps?
Please check in the source code constant `MinimumStakingAmount`.

### When developers register their dApp, can they registar WASM contract? (If not, can they update it in the future?)
The developers can register several dApps. But they need to use separate accounts and separate contract addresses.
The rule is

```1 developer <=> 1 contract```

### Does dApps staking supports Wasm contracts?
Yes.
Once the Wasm contracts are enabled on a parachain, Wasm contract could be used for dApps staking.
