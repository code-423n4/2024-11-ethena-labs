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
## 🐺 C4: Begin Gist paste here (and delete this line)





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

