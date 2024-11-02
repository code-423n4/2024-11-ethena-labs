# Ethena Labs audit details
- Total Prize Pool: $20,000 in USDC
  - HM awards: $15,800 in USDC*
  - QA awards: $700 in USDC*
  - Judge awards: $3,000 in USDC
  - Scout awards: $500 in USDC
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts November 4, 2024 20:00 UTC
- Ends November 11, 2024 20:00 UTC

ℹ️ If no valid HMs are found, the QA awards will increase to $7,700 in USDC.

*Note: This competition will be followed by a private Zenith mitigation review.*

## This is a Private audit

This audit repo and its Discord channel are accessible to **certified wardens only.** Participation in private audits is bound by:

1. Code4rena's [Certified Contributor Terms and Conditions](https://github.com/code-423n4/code423n4.com/blob/main/_data/pages/certified-contributor-terms-and-conditions.md)
2. C4's [Certified Contributor Code of Professional Conduct](https://code4rena.notion.site/Code-of-Professional-Conduct-657c7d80d34045f19eee510ae06fef55)

*All discussions regarding private audits should be considered private and confidential, unless otherwise indicated.*

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/4naly3er-report.md).

_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._
## 🐺 C4 team: paste this into the bottom of the sponsor's audit repo `README`, then delete this line

- Lack of storage gap in upgradeable base contract. This issue may introduce storage collisions for inheriting contracts. This issue can be mitigated by introducing custom storage slots in future upgrades should they be necessary.

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

# Overview

[ ⭐️ SPONSORS: add info here ]

## Links

- **Previous audits:**  three audit reports are available in the public contest repo shared
  - ✅ SCOUTS: If there are multiple report links, please format them in a list.
- **Documentation:** https://github.com/ethena-labs/ethena-ustb-contest/blob/main/README.md
- **Website:** https://ethena.fi/
- **X/Twitter:** https://twitter.com/ethena_labs
- **Discord:** https://discord.com/invite/ethena

---

# Scope

[ ✅ SCOUTS: add scoping and technical details here ]

### Files in scope
- ✅ This should be completed using the `metrics.md` file
- ✅ Last row of the table should be Total: SLOC
- ✅ SCOUTS: Have the sponsor review and and confirm in text the details in the section titled "Scoping Q amp; A"

*For sponsors that don't use the scoping tool: list all files in scope in the table below (along with hyperlinks) -- and feel free to add notes to emphasize areas of focus.*

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [contracts/folder/sample.sol](https://github.com/code-423n4/repo-name/blob/contracts/folder/sample.sol) | 123 | This contract does XYZ | [`@openzeppelin/*`](https://openzeppelin.com/contracts/) |

### Files out of scope
✅ SCOUTS: List files/directories out of scope

## Scoping Q &amp; A

### General questions
### Are there any ERC20's in scope?: Yes

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".

Specific tokens (please specify)
BUIDL, USDC - generally stable coins

### Are there any ERC777's in scope?: No

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".



### Are there any ERC721's in scope?: No

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".



### Are there any ERC1155's in scope?: No

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".



✅ SCOUTS: Once done populating the table below, please remove all the Q/A data above.

| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |       🖊️             |
| Test coverage                           | ✅ SCOUTS: Please populate this after running the test coverage command                          |
| ERC721 used  by the protocol            |            🖊️              |
| ERC777 used by the protocol             |           🖊️                |
| ERC1155 used by the protocol            |              🖊️            |
| Chains the protocol will be deployed on | Ethereum |

### ERC20 token behaviors in scope

| Question                                                                                                                                                   | Answer |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| [Missing return values](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#missing-return-values)                                                      |    |
| [Fee on transfer](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#fee-on-transfer)                                                                  |   |
| [Balance changes outside of transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#balance-modifications-outside-of-transfers-rebasingairdrops) |    |
| [Upgradeability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#upgradable-tokens)                                                                 |    |
| [Flash minting](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#flash-mintable-tokens)                                                              |    |
| [Pausability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#pausable-tokens)                                                                      |    |
| [Approval race protections](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#approval-race-protections)                                              |    |
| [Revert on approval to zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-approval-to-zero-address)                            |    |
| [Revert on zero value approvals](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-approvals)                                    |    |
| [Revert on zero value transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                    |    |
| [Revert on transfer to the zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-transfer-to-the-zero-address)                    |    |
| [Revert on large approvals and/or transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-large-approvals--transfers)                  |    |
| [Doesn't revert on failure](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#no-revert-on-failure)                                                   |    |
| [Multiple token addresses](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                          |    |
| [Low decimals ( < 6)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#low-decimals)                                                                 |    |
| [High decimals ( > 18)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#high-decimals)                                                              |    |
| [Blocklists](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#tokens-with-blocklists)                                                                |    |

### External integrations (e.g., Uniswap) behavior in scope:


| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | No   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   No  |


### EIP compliance checklist
- EIP 1271 for signing schema related to smart contracts signing an order

✅ SCOUTS: Please format the response above 👆 using the template below👇

| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| src/Token.sol                           | ERC20, ERC721                |
| src/NFT.sol                             | ERC721                       |


# Additional context

## Main invariants

The main invariants are already detailed in the readme.

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## Attack ideas (where to focus for bugs)
- a blacklisted user circumventing token transfer restrictions in any of the transfer states defined in UStb token contract.
- any user circumventing token transfers in a FULLY_DISABLED transfer state in UStb token contract.
- collateral being accessed in an unexpected way in the UStb minting contract

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## All trusted roles in the protocol

- DEFAULT_ADMIN - can assign roles to themselves or others and has the ability to perform any action
- MINTER - has the ability to call the mint function in UStb minting contract
- REDEEMER - has the ability to call the redeem function in UStb minting contract
- GATEKEEPER - has the ability to disable minting and redeeming
- BLACKLIST_MANAGER - has the ability to blacklist addresses
- WHITELIST_MANAGER - has the ability to whitelist addresses

✅ SCOUTS: Please format the response above 👆 using the template below👇

| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| Owner                          | Has superpowers                |
| Administrator                             | Can change fees                       |

## Describe any novel or unique curve logic or mathematical models implemented in the contracts:

N/A

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## Running tests

forge build
forge test --gas-report

✅ SCOUTS: Please format the response above 👆 using the template below👇

```bash
git clone https://github.com/code-423n4/2023-08-arbitrum
git submodule update --init --recursive
cd governance
foundryup
make install
make build
make sc-election-test
```
To run code coverage
```bash
make coverage
```
To run gas benchmarks
```bash
make gas
```

✅ SCOUTS: Add a screenshot of your terminal showing the gas report
✅ SCOUTS: Add a screenshot of your terminal showing the test coverage






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

| File         |
| ------------ |
| ./contracts/interfaces/ISingleAdminAccessControl.sol |
| ./contracts/lib/Upgrades.sol |
| ./contracts/mock/MockMultisigWallet.sol |
| ./contracts/mock/MockToken.sol |
| ./contracts/mock/MockUSDT.sol |
| ./contracts/ustb/IUStb.sol |
| ./contracts/ustb/IUStbDefinitions.sol |
| ./contracts/ustb/IUStbMinting.sol |
| ./contracts/ustb/IUStbMintingEvents.sol |
| ./test/foundry/UStb.admin.t.sol |
| ./test/foundry/UStb.transfers.t.sol |
| ./test/foundry/UStbBaseSetup.sol |
| ./test/foundry/UStbMinting.utils.sol |
| ./test/foundry/UStbMintingBaseSetup.sol |
| ./test/foundry/test/UStbMinting.ACL.t.sol |
| ./test/foundry/test/UStbMinting.Delegate.t.sol |
| ./test/foundry/test/UStbMinting.SmartContractSigning.t.sol |
| ./test/foundry/test/UStbMinting.StableRatios.t.sol |
| ./test/foundry/test/UStbMinting.Whitelist.t.sol |
| ./test/foundry/test/UStbMinting.blockLimits.t.sol |
| ./test/foundry/test/UStbMinting.core.t.sol |
| ./test/utils/SigUtils.sol |
| ./test/utils/Test.sol |
| ./test/utils/Utils.sol |
| Totals: 24 |

## Miscellaneous
Employees of Ethena Labs and employees' family members are ineligible to participate in this audit.

Code4rena's rules cannot be overridden by the contents of this README. In case of doubt, please check with C4 staff.
