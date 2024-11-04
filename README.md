# Ethena Labs Invitational audit details
- Total Prize Pool: $20,000 in USDC
  - HM awards: $15,800 in USDC*
  - QA awards: $700 in USDC*
  - Judge awards: $3,000 in USDC
  - Scout awards: $500 in USDC
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts November 4, 2024 20:00 UTC
- Ends November 11, 2024 20:00 UTC

ℹ️ If no valid HMs are found, the QA awards will increase to &#36;7,700 in USDC.

*Note: This competition will be followed by a private Zenith mitigation review.*

## This is a Private audit

This audit repo and its Discord channel are accessible to **certified wardens only.** Participation in private audits is bound by:

1. Code4rena's [Certified Contributor Terms and Conditions](https://github.com/code-423n4/code423n4.com/blob/main/_data/pages/certified-contributor-terms-and-conditions.md)
2. C4's [Certified Contributor Code of Professional Conduct](https://code4rena.notion.site/Code-of-Professional-Conduct-657c7d80d34045f19eee510ae06fef55)

*All discussions regarding private audits should be considered private and confidential, unless otherwise indicated.*

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/4naly3er-report.md).

_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

- Lack of storage gap in upgradeable base contract. This issue may introduce storage collisions for inheriting contracts. This issue can be mitigated by introducing custom storage slots in future upgrades should they be necessary.

- USDC can blacklist an arbitrary address which could could interfer with the minting/redeeming process.

- BUIDL can seize funds from an arbitrary address.

- Centralization risk in relation to the various actors in the contracts.

- Unsafe cast from uint128 to uint64 can cause collisions for the nonce in `verifyNonce` function

# Overview

## UStb token and minting

### UStb token features

**Overview**: An upgradeable ERC20 with mint and burn functionality and various transfer states that is controlled by a single admin address.

#### 1. Whitelisting

A set of addresses that are whitelisted for the purpose of transfer restrictions. Only the whitelist manager specified by the admin can add or remove whitelist addresses.

#### 2. Blacklisting

A set of addresses that are blacklisted for the purpose of transfer restrictions. In any case blacklisted addresses cannot send or receive tokens. Only the blacklist manager specified by the admin can add or remove blacklisted addresses.

#### 3. Token Redistribution

Allows the admin to forcefully move tokens from a blacklisted address to a non-blacklisted address.

#### 4. Transfer States

The admin address can change the state at any time, without a timelock. There are three main transfer states to consider:

- `FULLY_DISABLED`: No holder of this token, whether whitelisted, blacklisted or otherwise can send or receive this token.
- `WHITELIST_ENABLED`: Only whitelisted addresses can send and receive this token.
- `FULLY_ENABLED`: Only non-blacklisted addresses can send and receive this token.

### UStb minting features

**Overview**: A contract defining the operations to mint and redeem UStb tokens based on signed orders that is controlled by a single admin. The price present in any mint/redeem orders are determined by an off-chain RFQ system controlled by Ethena, which a benefactor may accept and sign an order for. The minter/redeemer then has last look rights to be able to filter out any malicious orders and proceed with on-chain settlement.

#### 1. Max mint/redeem per block by collateral

Implements the max amount of UStb that can be minted/redeemed in a single block using a certain type of collateral. The limit can be adjusted by the admin on a per collateral basis, regardless whether the collateral is active or not.

#### 2. Global max mint/redeem per block

In addition to mint/redeem limits by collateral, there is a global mint/redeem per block configuration that caps the amount of UStb that can be minted in a single block, regardless of the collateral used to mint UStb. The admin can adjust this configurations, regardless whether the collateral is active or not.

#### 3. Delegate signer

Allows an address to delegate signing to another address. The mechanism to set a delegate signer is a two-step process, first the delegator needs to propose a delegatee, finally the delegatee needs to accept the role. The purpose of this feature is to allow smart contracts to delegate signing to an EOA to sign mint/redeem instructions.

#### 4. Custodians

Custodians are the only addresses that can receive collateral assets from the mint process.

#### 5. Benefactor

An address holding collateral assets (benefactor) for a minting instruction that can receive UStb from the minting process. Benefactors are entities that have undergone KYC with Ethena and have been expressly registered by the admin to be able to participate in mint/redeem operations.

#### 6. Beneficiary

An address holding collateral assets (benefactor) for a minting instruction can assign a different address (beneficiary) to receive UStb.

#### 6. TokenType

A collateral can be assigned `STABLE` or `ASSET` token type. Depending on the token type `verifyStablesLimit` will be called during minting/redeeming operations which provide restrictions to price discrepencies in the order.

## Links

- **Previous audits:**  three audit reports are available in the public contest repo shared
  - [Pashov Audit Group](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/audits/Ethena-security-review-October.pdf)
  - [Quanstamp](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/audits/Ethena_final_report_Quantstamp.pdf)
  - [Cyfrin](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/audits/2024-10-31-ethena-ustb-v1.0.pdf)
- **Documentation:** Found in the [Overview](#overview) scetion above
- **Website:** https://ethena.fi/
- **X/Twitter:** https://twitter.com/ethena_labs
- **Discord:** https://discord.com/invite/ethena

---


# Scope

*See [scope.txt](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/scope.txt)*

### Files in scope


| File   | Logic Contracts | Interfaces | nSLOC | Purpose | Libraries used |
| ------ | --------------- | ---------- | ----- | -----   | ------------ |
| /contracts/SingleAdminAccessControl.sol | 1| **** | 42 | |@openzeppelin/contracts/access/AccessControl.sol<br>@openzeppelin/contracts/interfaces/IERC5313.sol|
| /contracts/SingleAdminAccessControlUpgradeable.sol | 1| **** | 42 | |@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol<br>@openzeppelin/contracts/interfaces/IERC5313.sol|
| /contracts/ustb/UStb.sol | 1| **** | 127 | |@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20PermitUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20BurnableUpgradeable.sol<br>@openzeppelin/contracts-upgradeable/token/ERC20/utils/SafeERC20Upgradeable.sol<br>@openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol|
| /contracts/ustb/UStbMinting.sol | 1| **** | 454 | |@openzeppelin/contracts/security/ReentrancyGuard.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts/utils/cryptography/ECDSA.sol<br>@openzeppelin/contracts/utils/structs/EnumerableSet.sol<br>@openzeppelin/contracts/interfaces/IERC1271.sol|
| **Totals** | **4** | **** | **665** | | |

### Files out of scope

*See [out_of_scope.txt](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/out_of_scope.txt)*


## Scoping Q &amp; A

### General questions



| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |      BUIDL, USDC             |
| Test coverage                           | 87.56% (373/426 statements)                          |
| ERC721 used  by the protocol            |            None              |
| ERC777 used by the protocol             |           None                |
| ERC1155 used by the protocol            |              None            |
| Chains the protocol will be deployed on | Ethereum |

### External integrations (e.g., Uniswap) behavior in scope:


| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | No   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   No  |


### EIP compliance checklist
- EIP 1271 for signing schema related to smart contracts signing an order


# Additional context

## Main invariants
- Blacklisted users cannot send/receive/mint/burn UStb tokens in any case.
- Only whitelisted user can send/receive/burn UStb tokens in a `WHITELIST_ENABLED` transfer state.
- Only non-blacklisted addresses can send/receive/burn UStb tokens in a `FULLY_ENABLED` transfer state.
- No adresses can send/receive tokens in a `FULLY_DISABLED` transfer state.
- Only the `MINTER` can mint UStb in any case.

## Attack ideas (where to focus for bugs)
- a blacklisted user circumventing token transfer restrictions in any of the transfer states defined in UStb token contract.
- any user circumventing token transfers in a FULLY_DISABLED transfer state in UStb token contract.
- collateral being accessed in an unexpected way from the UStb minting contract


## All trusted roles in the protocol



| Role              | Description                                                                        |
|-------------------|------------------------------------------------------------------------------------|
| DEFAULT_ADMIN     | can assign roles to themselves or others and has the ability to perform any action |
| MINTER            | has the ability to call the mint function in UStb minting contract                 |
| REDEEMER          | has the ability to call the redeem function in UStb minting contract               |
| GATEKEEPER        | has the ability to disable minting and redeeming                                   |
| BLACKLIST_MANAGER | has the ability to blacklist addresses                                             |
| WHITELIST_MANAGER | has the ability to whitelist addresses                                             |
| COLLATERAL_MANAGER| has the ability to withdraw collateral to custodians                               |


## Describe any novel or unique curve logic or mathematical models implemented in the contracts:

N/A


## Running tests

```bash
foundryup

git clone https://github.com/code-423n4/2024-11-ethena-labs.git
cd 2024-11-ethena-labs
forge test
```
To run code coverage
```bash
forge coverage
```


| File                                              | % Lines          | % Statements     | % Branches      | % Funcs         |
|---------------------------------------------------|------------------|------------------|-----------------|-----------------|
| contracts/SingleAdminAccessControl.sol            | 100.00% (16/16)  | 94.74% (18/19)   | 75.00% (3/4)    | 100.00% (8/8)   |
| contracts/SingleAdminAccessControlUpgradeable.sol | 56.25% (9/16)    | 47.37% (9/19)    | 25.00% (1/4)    | 50.00% (4/8)    |
| contracts/ustb/UStb.sol                           | 87.10% (54/62)   | 93.91% (108/115) | 53.33% (16/30)  | 85.71% (12/14)  |
| contracts/ustb/UStbMinting.sol                    | 91.09% (184/202) | 87.18% (238/273) | 63.49% (40/63)  | 81.25% (39/48)  |
| Total                                       | 88.85% (263/296) | 87.56% (373/426) | 59.41% (60/101)  | 80.77% (63/78) |


## Miscellaneous
Employees of Ethena Labs and employees' family members are ineligible to participate in this audit.

Code4rena's rules cannot be overridden by the contents of this README. In case of doubt, please check with C4 staff.
