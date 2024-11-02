# Report


## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings) | 6 |
| [GAS-2](#GAS-2) | Cache array length outside of loop | 8 |
| [GAS-3](#GAS-3) | For Operations that will not overflow, you could use unchecked | 48 |
| [GAS-4](#GAS-4) | Avoid contract existence checks by using low level calls | 1 |
| [GAS-5](#GAS-5) | Functions guaranteed to revert when called by normal users can be marked `payable` | 32 |
| [GAS-6](#GAS-6) | `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`) | 4 |
| [GAS-7](#GAS-7) | Using `private` rather than `public` for constants, saves gas | 5 |
| [GAS-8](#GAS-8) | Increments/decrements can be unchecked in for-loops | 4 |
| [GAS-9](#GAS-9) | Use != 0 instead of > 0 for unsigned integer comparison | 1 |
### <a name="GAS-1"></a>[GAS-1] `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings)
This saves **16 gas per instance.**

*Instances (6)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

235:         totalPerBlockPerAsset[block.number][order.collateral_asset].mintedPerBlock += order.ustb_amount;

236:         totalPerBlock[block.number].mintedPerBlock += order.ustb_amount;

276:         totalPerBlockPerAsset[block.number][order.collateral_asset].redeemedPerBlock += order.ustb_amount;

277:         totalPerBlock[block.number].redeemedPerBlock += order.ustb_amount;

525:             totalRatio += route.ratios[i];

614:             totalTransferred += amountToTransfer;

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="GAS-2"></a>[GAS-2] Cache array length outside of loop
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (8)*:
```solidity
File: ./contracts/ustb/UStb.sol

74:         for (uint8 i = 0; i < users.length; i++) {

83:         for (uint8 i = 0; i < users.length; i++) {

92:         for (uint8 i = 0; i < users.length; i++) {

101:         for (uint8 i = 0; i < users.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

175:         for (uint128 j = 0; j < _custodians.length; ) {

186:         for (uint128 k = 0; k < _tokenConfig.length; ) {

517:         for (uint128 i = 0; i < route.addresses.length; ) {

611:         for (uint128 i = 0; i < addresses.length; ) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="GAS-3"></a>[GAS-3] For Operations that will not overflow, you could use unchecked

*Instances (48)*:
```solidity
File: ./contracts/SingleAdminAccessControl.sol

4: import "@openzeppelin/contracts/access/AccessControl.sol";

5: import "@openzeppelin/contracts/interfaces/IERC5313.sol";

6: import "./interfaces/ISingleAdminAccessControl.sol";

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControl.sol)

```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

4: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

5: import "@openzeppelin/contracts/interfaces/IERC5313.sol";

6: import "./interfaces/ISingleAdminAccessControl.sol";

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20PermitUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20BurnableUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/utils/SafeERC20Upgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol";

8: import "../SingleAdminAccessControlUpgradeable.sol";

9: import "./IUStbDefinitions.sol";

74:         for (uint8 i = 0; i < users.length; i++) {

83:         for (uint8 i = 0; i < users.length; i++) {

92:         for (uint8 i = 0; i < users.length; i++) {

101:         for (uint8 i = 0; i < users.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

7: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

8: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

9: import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

10: import "@openzeppelin/contracts/utils/structs/EnumerableSet.sol";

11: import "@openzeppelin/contracts/interfaces/IERC1271.sol";

13: import "./IUStbMinting.sol";

14: import "./IUStb.sol";

15: import "../SingleAdminAccessControl.sol";

116:         if (totalPerBlockPerAsset[block.number][asset].mintedPerBlock + mintAmount > _config.maxMintPerBlock) {

128:         if (totalPerBlockPerAsset[block.number][asset].redeemedPerBlock + redeemAmount > _config.maxRedeemPerBlock) {

139:         if (totalMintedThisBlock + mintAmount > globalConfig.globalMaxMintPerBlock)

149:         if (totalRedeemedThisBlock + redeemAmount > globalConfig.globalMaxRedeemPerBlock) {

178:                 ++j;

192:                 ++k;

235:         totalPerBlockPerAsset[block.number][order.collateral_asset].mintedPerBlock += order.ustb_amount;

236:         totalPerBlock[block.number].mintedPerBlock += order.ustb_amount;

276:         totalPerBlockPerAsset[block.number][order.collateral_asset].redeemedPerBlock += order.ustb_amount;

277:         totalPerBlock[block.number].redeemedPerBlock += order.ustb_amount;

525:             totalRatio += route.ratios[i];

527:                 ++i;

556:                 ? 10 ** (ustbDecimals - collateralDecimals)

557:                 : 10 ** (collateralDecimals - ustbDecimals)

561:             ? collateralAmount * scale

562:             : collateralAmount / scale;

565:             ? normalizedCollateralAmount - ustbAmount

566:             : ustbAmount - normalizedCollateralAmount;

568:         uint128 differenceInBps = (difference * STABLES_RATIO_MULTIPLIER) / ustbAmount;

612:             uint128 amountToTransfer = (amount * ratios[i]) / ROUTE_REQUIRED_RATIO;

614:             totalTransferred += amountToTransfer;

616:                 ++i;

619:         uint128 remainingBalance = amount - totalTransferred;

621:             token.safeTransferFrom(benefactor, addresses[addresses.length - 1], remainingBalance);

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="GAS-4"></a>[GAS-4] Avoid contract existence checks by using low level calls
Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

*Instances (1)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

465:             address signer = ECDSA.recover(taker_order_hash, signature.signature_bytes);

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="GAS-5"></a>[GAS-5] Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (32)*:
```solidity
File: ./contracts/SingleAdminAccessControl.sol

25:     function transferAdmin(address newAdmin) external onlyRole(DEFAULT_ADMIN_ROLE) {

41:     function grantRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

50:     function revokeRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControl.sol)

```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

25:     function transferAdmin(address newAdmin) external onlyRole(DEFAULT_ADMIN_ROLE) {

41:     function grantRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

50:     function revokeRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

59:     function addMinter(address minterContract) external onlyRole(DEFAULT_ADMIN_ROLE) {

64:     function removeMinter(address minterContract) external onlyRole(DEFAULT_ADMIN_ROLE) {

73:     function addBlacklistAddress(address[] calldata users) external onlyRole(BLACKLIST_MANAGER_ROLE) {

82:     function removeBlacklistAddress(address[] calldata users) external onlyRole(BLACKLIST_MANAGER_ROLE) {

91:     function addWhitelistAddress(address[] calldata users) external onlyRole(WHITELIST_MANAGER_ROLE) {

100:     function removeWhitelistAddress(address[] calldata users) external onlyRole(WHITELIST_MANAGER_ROLE) {

111:     function redistributeLockedAmount(address from, address to) external nonReentrant onlyRole(DEFAULT_ADMIN_ROLE) {

143:     function mint(address to, uint256 amount) external onlyRole(MINTER_CONTRACT) {

159:     function updateTransferState(TransferState code) external onlyRole(DEFAULT_ADMIN_ROLE) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

292:     function setGlobalMaxMintPerBlock(uint128 _globalMaxMintPerBlock) external onlyRole(DEFAULT_ADMIN_ROLE) {

298:     function setGlobalMaxRedeemPerBlock(uint128 _globalMaxRedeemPerBlock) external onlyRole(DEFAULT_ADMIN_ROLE) {

304:     function disableMintRedeem() external onlyRole(GATEKEEPER_ROLE) {

348:     function removeSupportedAsset(address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

360:     function removeCustodianAddress(address custodian) external onlyRole(DEFAULT_ADMIN_ROLE) {

367:     function removeMinterRole(address minter) external onlyRole(GATEKEEPER_ROLE) {

373:     function removeRedeemerRole(address redeemer) external onlyRole(GATEKEEPER_ROLE) {

379:     function removeCollateralManagerRole(address collateralManager) external onlyRole(GATEKEEPER_ROLE) {

384:     function removeWhitelistedBenefactor(address benefactor) external onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function addCustodianAddress(address custodian) public onlyRole(DEFAULT_ADMIN_ROLE) {

400:     function addWhitelistedBenefactor(address benefactor) public onlyRole(DEFAULT_ADMIN_ROLE) {

633:     function addSupportedAsset(address asset, TokenConfig memory _tokenConfig) external onlyRole(DEFAULT_ADMIN_ROLE) {

641:     function setMaxMintPerBlock(uint128 _maxMintPerBlock, address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

651:     function setMaxRedeemPerBlock(uint128 _maxRedeemPerBlock, address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

669:     function setTokenType(address asset, TokenType tokenType) external onlyRole(DEFAULT_ADMIN_ROLE) {

676:     function setStablesDeltaLimit(uint128 _stablesDeltaLimit) external onlyRole(DEFAULT_ADMIN_ROLE) {

681:     function setUStbToken(IUStb _ustb) external onlyRole(DEFAULT_ADMIN_ROLE) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="GAS-6"></a>[GAS-6] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)
Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name *post-increment*:

```solidity
uint i = 1;  
uint j = 2;
require(j == i++, "This will be false as i is incremented after the comparison");
```
  
However, pre-increments (or pre-decrements) return the new value:
  
```solidity
uint i = 1;  
uint j = 2;
require(j == ++i, "This will be true as i is incremented before the comparison");
```

In the pre-increment case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`.

Consider using pre-increments and pre-decrements where they are relevant (meaning: not where post-increments/decrements logic are relevant).

*Saves 5 gas per instance*

*Instances (4)*:
```solidity
File: ./contracts/ustb/UStb.sol

74:         for (uint8 i = 0; i < users.length; i++) {

83:         for (uint8 i = 0; i < users.length; i++) {

92:         for (uint8 i = 0; i < users.length; i++) {

101:         for (uint8 i = 0; i < users.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

### <a name="GAS-7"></a>[GAS-7] Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*Instances (5)*:
```solidity
File: ./contracts/ustb/UStb.sol

25:     bytes32 public constant MINTER_CONTRACT = keccak256("MINTER_CONTRACT");

27:     bytes32 public constant BLACKLIST_MANAGER_ROLE = keccak256("BLACKLIST_MANAGER_ROLE");

29:     bytes32 public constant WHITELIST_MANAGER_ROLE = keccak256("WHITELIST_MANAGER_ROLE");

31:     bytes32 public constant BLACKLISTED_ROLE = keccak256("BLACKLISTED_ROLE");

33:     bytes32 public constant WHITELISTED_ROLE = keccak256("WHITELISTED_ROLE");

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

### <a name="GAS-8"></a>[GAS-8] Increments/decrements can be unchecked in for-loops
In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

[ethereum/solidity#10695](https://github.com/ethereum/solidity/issues/10695)

The change would be:

```diff
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...  
+   unchecked { ++i; }
}  
```

These save around **25 gas saved** per instance.

The same can be applied with decrements (which should use `break` when `i == 0`).

The risk of overflow is non-existent for `uint256`.

*Instances (4)*:
```solidity
File: ./contracts/ustb/UStb.sol

74:         for (uint8 i = 0; i < users.length; i++) {

83:         for (uint8 i = 0; i < users.length; i++) {

92:         for (uint8 i = 0; i < users.length; i++) {

101:         for (uint8 i = 0; i < users.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

### <a name="GAS-9"></a>[GAS-9] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (1)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

620:         if (remainingBalance > 0) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)


## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | `constant`s should be defined rather than using magic numbers | 3 |
| [NC-2](#NC-2) | Control structures do not follow the Solidity Style Guide | 50 |
| [NC-3](#NC-3) | Critical Changes Should Use Two-step Procedure | 3 |
| [NC-4](#NC-4) | Functions should not be longer than 50 lines | 61 |
| [NC-5](#NC-5) | Lines are too long | 1 |
| [NC-6](#NC-6) | Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor | 15 |
| [NC-7](#NC-7) | Consider using named mappings | 6 |
| [NC-8](#NC-8) | `address`s shouldn't be hard-coded | 1 |
| [NC-9](#NC-9) | Take advantage of Custom Error's return value property | 54 |
| [NC-10](#NC-10) | Avoid the use of sensitive terms | 41 |
| [NC-11](#NC-11) | Use Underscores for Number Literals (add an underscore every 3 digits) | 1 |
| [NC-12](#NC-12) | Variables need not be initialized to zero | 10 |
### <a name="NC-1"></a>[NC-1] `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*Instances (3)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

536:         uint128 invalidatorSlot = uint64(nonce) >> 8;

556:                 ? 10 ** (ustbDecimals - collateralDecimals)

557:                 : 10 ** (collateralDecimals - ustbDecimals)

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-2"></a>[NC-2] Control structures do not follow the Solidity Style Guide
See the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) section of the Solidity Style Guide

*Instances (50)*:
```solidity
File: ./contracts/SingleAdminAccessControl.sol

18:         if (role == DEFAULT_ADMIN_ROLE) revert InvalidAdminChange();

26:         if (newAdmin == msg.sender) revert InvalidAdminChange();

32:         if (msg.sender != _pendingDefaultAdmin) revert NotPendingAdmin();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControl.sol)

```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

18:         if (role == DEFAULT_ADMIN_ROLE) revert InvalidAdminChange();

26:         if (newAdmin == msg.sender) revert InvalidAdminChange();

32:         if (msg.sender != _pendingDefaultAdmin) revert NotPendingAdmin();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

52:         if (admin == address(0) || minterContract == address(0)) revert ZeroAddressException();

174:             } else if (

178:             } else if (

195:             } else if (

201:             } else if (

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

29:         keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)");

115:         if (!_config.isActive) revert UnsupportedAsset();

127:         if (!_config.isActive) revert UnsupportedAsset();

139:         if (totalMintedThisBlock + mintAmount > globalConfig.globalMaxMintPerBlock)

164:         if (_tokenConfig.length == 0) revert NoAssetsProvided();

165:         if (_assets.length == 0) revert NoAssetsProvided();

166:         if (_admin == address(0)) revert InvalidZeroAddress();

230:         if (order.order_type != OrderType.MINT) revert InvalidOrder();

231:         verifyOrder(order, signature);

232:         if (!verifyRoute(route)) revert InvalidRoute();

272:         if (order.order_type != OrderType.REDEEM) revert InvalidOrder();

273:         verifyOrder(order, signature);

337:         if (wallet == address(0) || !_custodianAddresses.contains(wallet)) revert InvalidAddress();

340:             if (!success) revert TransferFailed();

349:         if (!tokenConfig[asset].isActive) revert InvalidAssetAddress();

361:         if (!_custodianAddresses.remove(custodian)) revert InvalidCustodianAddress();

385:         if (!_whitelistedBenefactors.remove(benefactor)) revert InvalidAddress();

459:     function verifyOrder(

466:             if (

473:             if (

492:             if (

493:                 !verifyStablesLimit(

503:         if (order.beneficiary == address(0)) revert InvalidAddress();

504:         if (order.collateral_amount == 0 || order.ustb_amount == 0) revert InvalidAmount();

505:         if (block.timestamp > order.expiry) revert SignatureExpired();

518:             if (

535:         if (nonce == 0) revert InvalidNonce();

539:         if (invalidator & invalidatorBit != 0) revert InvalidNonce();

544:     function verifyStablesLimit(

564:         uint128 difference = normalizedCollateralAmount > ustbAmount

568:         uint128 differenceInBps = (difference * STABLES_RATIO_MULTIPLIER) / ustbAmount;

571:             return ustbAmount > normalizedCollateralAmount ? differenceInBps <= stablesDeltaLimit : true;

573:             return normalizedCollateralAmount > ustbAmount ? differenceInBps <= stablesDeltaLimit : true;

581:         (uint128 invalidatorSlot, uint256 invalidator, uint256 invalidatorBit) = verifyNonce(sender, nonce);

590:             if (address(this).balance < amount) revert InvalidAmount();

592:             if (!success) revert TransferFailed();

594:             if (!tokenConfig[asset].isActive) revert UnsupportedAsset();

608:         if (!tokenConfig[asset].isActive || asset == NATIVE_TOKEN) revert UnsupportedAsset();

670:         if (!tokenConfig[asset].isActive) revert UnsupportedAsset();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-3"></a>[NC-3] Critical Changes Should Use Two-step Procedure
The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference: <https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure>

**Recommended Mitigation Steps**

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

*Instances (3)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

641:     function setMaxMintPerBlock(uint128 _maxMintPerBlock, address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

651:     function setMaxRedeemPerBlock(uint128 _maxRedeemPerBlock, address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

669:     function setTokenType(address asset, TokenType tokenType) external onlyRole(DEFAULT_ADMIN_ROLE) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-4"></a>[NC-4] Functions should not be longer than 50 lines
Overly complex code can make understanding functionality more difficult, try to further modularize your code to ensure readability 

*Instances (61)*:
```solidity
File: ./contracts/SingleAdminAccessControl.sol

25:     function transferAdmin(address newAdmin) external onlyRole(DEFAULT_ADMIN_ROLE) {

41:     function grantRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

50:     function revokeRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

58:     function renounceRole(bytes32 role, address account) public virtual override notAdmin(role) {

65:     function owner() public view virtual returns (address) {

72:     function _grantRole(bytes32 role, address account) internal override {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControl.sol)

```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

25:     function transferAdmin(address newAdmin) external onlyRole(DEFAULT_ADMIN_ROLE) {

41:     function grantRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

50:     function revokeRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

58:     function renounceRole(bytes32 role, address account) public virtual override notAdmin(role) {

65:     function owner() public view virtual returns (address) {

72:     function _grantRole(bytes32 role, address account) internal override {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

48:     function initialize(address admin, address minterContract) public initializer {

59:     function addMinter(address minterContract) external onlyRole(DEFAULT_ADMIN_ROLE) {

64:     function removeMinter(address minterContract) external onlyRole(DEFAULT_ADMIN_ROLE) {

73:     function addBlacklistAddress(address[] calldata users) external onlyRole(BLACKLIST_MANAGER_ROLE) {

82:     function removeBlacklistAddress(address[] calldata users) external onlyRole(BLACKLIST_MANAGER_ROLE) {

91:     function addWhitelistAddress(address[] calldata users) external onlyRole(WHITELIST_MANAGER_ROLE) {

100:     function removeWhitelistAddress(address[] calldata users) external onlyRole(WHITELIST_MANAGER_ROLE) {

111:     function redistributeLockedAmount(address from, address to) external nonReentrant onlyRole(DEFAULT_ADMIN_ROLE) {

143:     function mint(address to, uint256 amount) external onlyRole(MINTER_CONTRACT) {

152:     function renounceRole(bytes32, address) public virtual override {

159:     function updateTransferState(TransferState code) external onlyRole(DEFAULT_ADMIN_ROLE) {

165:     function _beforeTokenTransfer(address from, address to, uint256) internal virtual override {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

292:     function setGlobalMaxMintPerBlock(uint128 _globalMaxMintPerBlock) external onlyRole(DEFAULT_ADMIN_ROLE) {

298:     function setGlobalMaxRedeemPerBlock(uint128 _globalMaxRedeemPerBlock) external onlyRole(DEFAULT_ADMIN_ROLE) {

304:     function disableMintRedeem() external onlyRole(GATEKEEPER_ROLE) {

311:     function setDelegatedSigner(address _delegateTo) external {

317:     function confirmDelegatedSigner(address _delegatedBy) external {

326:     function removeDelegatedSigner(address _removedSigner) external {

348:     function removeSupportedAsset(address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

355:     function isSupportedAsset(address asset) external view returns (bool) {

360:     function removeCustodianAddress(address custodian) external onlyRole(DEFAULT_ADMIN_ROLE) {

367:     function removeMinterRole(address minter) external onlyRole(GATEKEEPER_ROLE) {

373:     function removeRedeemerRole(address redeemer) external onlyRole(GATEKEEPER_ROLE) {

379:     function removeCollateralManagerRole(address collateralManager) external onlyRole(GATEKEEPER_ROLE) {

384:     function removeWhitelistedBenefactor(address benefactor) external onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function addCustodianAddress(address custodian) public onlyRole(DEFAULT_ADMIN_ROLE) {

400:     function addWhitelistedBenefactor(address benefactor) public onlyRole(DEFAULT_ADMIN_ROLE) {

411:     function setApprovedBeneficiary(address beneficiary, bool status) public {

430:     function getDomainSeparator() public view returns (bytes32) {

438:     function hashOrder(Order calldata order) public view override returns (bytes32) {

442:     function encodeOrder(Order calldata order) public pure returns (bytes memory) {

509:     function verifyRoute(Route calldata route) public view override returns (bool) {

534:     function verifyNonce(address sender, uint128 nonce) public view override returns (uint128, uint256, uint256) {

580:     function _deduplicateOrder(address sender, uint128 nonce) private {

588:     function _transferToBeneficiary(address beneficiary, address asset, uint128 amount) internal {

625:     function _setTokenConfig(address asset, TokenConfig memory _tokenConfig) internal {

633:     function addSupportedAsset(address asset, TokenConfig memory _tokenConfig) external onlyRole(DEFAULT_ADMIN_ROLE) {

641:     function setMaxMintPerBlock(uint128 _maxMintPerBlock, address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

645:     function _setMaxMintPerBlock(uint128 _maxMintPerBlock, address asset) internal {

651:     function setMaxRedeemPerBlock(uint128 _maxRedeemPerBlock, address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

656:     function _setMaxRedeemPerBlock(uint128 _maxRedeemPerBlock, address asset) internal {

664:     function _computeDomainSeparator() internal view returns (bytes32) {

669:     function setTokenType(address asset, TokenType tokenType) external onlyRole(DEFAULT_ADMIN_ROLE) {

676:     function setStablesDeltaLimit(uint128 _stablesDeltaLimit) external onlyRole(DEFAULT_ADMIN_ROLE) {

681:     function setUStbToken(IUStb _ustb) external onlyRole(DEFAULT_ADMIN_ROLE) {

687:     function _getDecimals(address token) internal view returns (uint128) {

695:     function isCustodianAddress(address custodian) public view returns (bool) {

700:     function isWhitelistedBenefactor(address benefactor) public view returns (bool) {

705:     function isApprovedBeneficiary(address benefactor, address beneficiary) public view returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-5"></a>[NC-5] Lines are too long
Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over [164](https://github.com/aizatto/character-length) characters, the lines below should be split when they reach that length

*Instances (1)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

34:             "Order(string order_id,uint8 order_type,uint120 expiry,uint128 nonce,address benefactor,address beneficiary,address collateral_asset,uint128 collateral_amount,uint128 ustb_amount)"

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-6"></a>[NC-6] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor
If a function is supposed to be access-controlled, a `modifier` should be used instead of a `require/if` statement for more readability.

*Instances (15)*:
```solidity
File: ./contracts/SingleAdminAccessControl.sol

26:         if (newAdmin == msg.sender) revert InvalidAdminChange();

32:         if (msg.sender != _pendingDefaultAdmin) revert NotPendingAdmin();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControl.sol)

```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

26:         if (newAdmin == msg.sender) revert InvalidAdminChange();

32:         if (msg.sender != _pendingDefaultAdmin) revert NotPendingAdmin();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

168:             if (hasRole(MINTER_CONTRACT, msg.sender) && !hasRole(BLACKLISTED_ROLE, from) && to == address(0)) {

170:             } else if (hasRole(MINTER_CONTRACT, msg.sender) && from == address(0) && !hasRole(BLACKLISTED_ROLE, to)) {

172:             } else if (hasRole(DEFAULT_ADMIN_ROLE, msg.sender) && hasRole(BLACKLISTED_ROLE, from) && to == address(0)) {

189:             if (hasRole(MINTER_CONTRACT, msg.sender) && !hasRole(BLACKLISTED_ROLE, from) && to == address(0)) {

191:             } else if (hasRole(MINTER_CONTRACT, msg.sender) && from == address(0) && !hasRole(BLACKLISTED_ROLE, to)) {

193:             } else if (hasRole(DEFAULT_ADMIN_ROLE, msg.sender) && hasRole(BLACKLISTED_ROLE, from) && to == address(0)) {

199:             } else if (hasRole(WHITELISTED_ROLE, msg.sender) && hasRole(WHITELISTED_ROLE, from) && to == address(0)) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

196:         if (msg.sender != _admin) {

318:         if (delegatedSigner[msg.sender][_delegatedBy] != DelegatedSignerStatus.PENDING) {

413:             if (!_approvedBeneficiariesPerBenefactor[msg.sender].add(beneficiary)) {

419:             if (!_approvedBeneficiariesPerBenefactor[msg.sender].remove(beneficiary)) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-7"></a>[NC-7] Consider using named mappings
Consider moving to solidity version 0.8.18 or later, and using [named mappings](https://ethereum.stackexchange.com/questions/51629/how-to-name-the-arguments-in-mapping/145555#145555) to make it easier to understand the purpose of each mapping

*Instances (6)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

76:     mapping(address => EnumerableSet.AddressSet) private _approvedBeneficiariesPerBenefactor;

88:     mapping(address => mapping(uint256 => uint256)) private _orderBitmaps;

91:     mapping(address => mapping(address => DelegatedSignerStatus)) public delegatedSigner;

100:     mapping(uint256 => BlockTotals) public totalPerBlock;

103:     mapping(uint256 => mapping(address => BlockTotals)) public totalPerBlockPerAsset;

106:     mapping(address => TokenConfig) public tokenConfig;

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-8"></a>[NC-8] `address`s shouldn't be hard-coded
It is often better to declare `address`es as `immutable`, and assign them via constructor arguments. This allows the code to remain the same across deployments on different networks, and avoids recompilation when addresses need to change.

*Instances (1)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

53:     address private constant NATIVE_TOKEN = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-9"></a>[NC-9] Take advantage of Custom Error's return value property
An important feature of Custom Error is that values such as address, tokenID, msg.value can be written inside the () sign, this kind of approach provides a serious advantage in debugging and examining the revert details of dapps such as tenderly.

*Instances (54)*:
```solidity
File: ./contracts/SingleAdminAccessControl.sol

18:         if (role == DEFAULT_ADMIN_ROLE) revert InvalidAdminChange();

26:         if (newAdmin == msg.sender) revert InvalidAdminChange();

32:         if (msg.sender != _pendingDefaultAdmin) revert NotPendingAdmin();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControl.sol)

```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

18:         if (role == DEFAULT_ADMIN_ROLE) revert InvalidAdminChange();

26:         if (newAdmin == msg.sender) revert InvalidAdminChange();

32:         if (msg.sender != _pendingDefaultAdmin) revert NotPendingAdmin();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

52:         if (admin == address(0) || minterContract == address(0)) revert ZeroAddressException();

118:             revert OperationNotAllowed();

153:         revert OperationNotAllowed();

185:                 revert OperationNotAllowed();

212:                 revert OperationNotAllowed();

216:             revert OperationNotAllowed();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

115:         if (!_config.isActive) revert UnsupportedAsset();

117:             revert MaxMintPerBlockExceeded();

127:         if (!_config.isActive) revert UnsupportedAsset();

129:             revert MaxRedeemPerBlockExceeded();

140:             revert GlobalMaxMintPerBlockExceeded();

150:             revert GlobalMaxRedeemPerBlockExceeded();

164:         if (_tokenConfig.length == 0) revert NoAssetsProvided();

165:         if (_assets.length == 0) revert NoAssetsProvided();

166:         if (_admin == address(0)) revert InvalidZeroAddress();

172:             revert InvalidAssetAddress();

188:                 revert InvalidAssetAddress();

230:         if (order.order_type != OrderType.MINT) revert InvalidOrder();

232:         if (!verifyRoute(route)) revert InvalidRoute();

272:         if (order.order_type != OrderType.REDEEM) revert InvalidOrder();

319:             revert DelegationNotInitiated();

337:         if (wallet == address(0) || !_custodianAddresses.contains(wallet)) revert InvalidAddress();

340:             if (!success) revert TransferFailed();

349:         if (!tokenConfig[asset].isActive) revert InvalidAssetAddress();

361:         if (!_custodianAddresses.remove(custodian)) revert InvalidCustodianAddress();

385:         if (!_whitelistedBenefactors.remove(benefactor)) revert InvalidAddress();

394:             revert InvalidCustodianAddress();

402:             revert InvalidBenefactorAddress();

414:                 revert InvalidBeneficiaryAddress();

420:                 revert InvalidBeneficiaryAddress();

470:                 revert InvalidEIP712Signature();

477:                 revert InvalidEIP1271Signature();

480:             revert UnknownSignatureType();

483:             revert BenefactorNotWhitelisted();

487:                 revert BeneficiaryNotApproved();

500:                 revert InvalidStablePrice();

503:         if (order.beneficiary == address(0)) revert InvalidAddress();

504:         if (order.collateral_amount == 0 || order.ustb_amount == 0) revert InvalidAmount();

505:         if (block.timestamp > order.expiry) revert SignatureExpired();

535:         if (nonce == 0) revert InvalidNonce();

539:         if (invalidator & invalidatorBit != 0) revert InvalidNonce();

590:             if (address(this).balance < amount) revert InvalidAmount();

592:             if (!success) revert TransferFailed();

594:             if (!tokenConfig[asset].isActive) revert UnsupportedAsset();

608:         if (!tokenConfig[asset].isActive || asset == NATIVE_TOKEN) revert UnsupportedAsset();

627:             revert InvalidAmount();

635:             revert InvalidAssetAddress();

670:         if (!tokenConfig[asset].isActive) revert UnsupportedAsset();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-10"></a>[NC-10] Avoid the use of sensitive terms
Use [alternative variants](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/), e.g. allowlist/denylist instead of whitelist/blacklist

*Instances (41)*:
```solidity
File: ./contracts/ustb/UStb.sol

27:     bytes32 public constant BLACKLIST_MANAGER_ROLE = keccak256("BLACKLIST_MANAGER_ROLE");

29:     bytes32 public constant WHITELIST_MANAGER_ROLE = keccak256("WHITELIST_MANAGER_ROLE");

31:     bytes32 public constant BLACKLISTED_ROLE = keccak256("BLACKLISTED_ROLE");

33:     bytes32 public constant WHITELISTED_ROLE = keccak256("WHITELISTED_ROLE");

73:     function addBlacklistAddress(address[] calldata users) external onlyRole(BLACKLIST_MANAGER_ROLE) {

75:             _grantRole(BLACKLISTED_ROLE, users[i]);

82:     function removeBlacklistAddress(address[] calldata users) external onlyRole(BLACKLIST_MANAGER_ROLE) {

84:             _revokeRole(BLACKLISTED_ROLE, users[i]);

91:     function addWhitelistAddress(address[] calldata users) external onlyRole(WHITELIST_MANAGER_ROLE) {

93:             _grantRole(WHITELISTED_ROLE, users[i]);

100:     function removeWhitelistAddress(address[] calldata users) external onlyRole(WHITELIST_MANAGER_ROLE) {

102:             _revokeRole(WHITELISTED_ROLE, users[i]);

112:         if (hasRole(BLACKLISTED_ROLE, from) && !hasRole(BLACKLISTED_ROLE, to)) {

168:             if (hasRole(MINTER_CONTRACT, msg.sender) && !hasRole(BLACKLISTED_ROLE, from) && to == address(0)) {

170:             } else if (hasRole(MINTER_CONTRACT, msg.sender) && from == address(0) && !hasRole(BLACKLISTED_ROLE, to)) {

172:             } else if (hasRole(DEFAULT_ADMIN_ROLE, msg.sender) && hasRole(BLACKLISTED_ROLE, from) && to == address(0)) {

175:                 hasRole(DEFAULT_ADMIN_ROLE, msg.sender) && from == address(0) && !hasRole(BLACKLISTED_ROLE, to)

179:                 !hasRole(BLACKLISTED_ROLE, msg.sender) &&

180:                 !hasRole(BLACKLISTED_ROLE, from) &&

181:                 !hasRole(BLACKLISTED_ROLE, to)

188:         } else if (transferState == TransferState.WHITELIST_ENABLED) {

189:             if (hasRole(MINTER_CONTRACT, msg.sender) && !hasRole(BLACKLISTED_ROLE, from) && to == address(0)) {

191:             } else if (hasRole(MINTER_CONTRACT, msg.sender) && from == address(0) && !hasRole(BLACKLISTED_ROLE, to)) {

193:             } else if (hasRole(DEFAULT_ADMIN_ROLE, msg.sender) && hasRole(BLACKLISTED_ROLE, from) && to == address(0)) {

196:                 hasRole(DEFAULT_ADMIN_ROLE, msg.sender) && from == address(0) && !hasRole(BLACKLISTED_ROLE, to)

199:             } else if (hasRole(WHITELISTED_ROLE, msg.sender) && hasRole(WHITELISTED_ROLE, from) && to == address(0)) {

202:                 hasRole(WHITELISTED_ROLE, msg.sender) &&

203:                 hasRole(WHITELISTED_ROLE, from) &&

204:                 hasRole(WHITELISTED_ROLE, to) &&

205:                 !hasRole(BLACKLISTED_ROLE, msg.sender) &&

206:                 !hasRole(BLACKLISTED_ROLE, from) &&

207:                 !hasRole(BLACKLISTED_ROLE, to)

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

73:     EnumerableSet.AddressSet private _whitelistedBenefactors;

384:     function removeWhitelistedBenefactor(address benefactor) external onlyRole(DEFAULT_ADMIN_ROLE) {

385:         if (!_whitelistedBenefactors.remove(benefactor)) revert InvalidAddress();

400:     function addWhitelistedBenefactor(address benefactor) public onlyRole(DEFAULT_ADMIN_ROLE) {

401:         if (benefactor == address(0) || !_whitelistedBenefactors.add(benefactor)) {

482:         if (!_whitelistedBenefactors.contains(order.benefactor)) {

483:             revert BenefactorNotWhitelisted();

700:     function isWhitelistedBenefactor(address benefactor) public view returns (bool) {

701:         return _whitelistedBenefactors.contains(benefactor);

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-11"></a>[NC-11] Use Underscores for Number Literals (add an underscore every 3 digits)

*Instances (1)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

65:     uint128 private constant STABLES_RATIO_MULTIPLIER = 10000;

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="NC-12"></a>[NC-12] Variables need not be initialized to zero
The default value for variables is zero, so initializing them to zero is superfluous.

*Instances (10)*:
```solidity
File: ./contracts/ustb/UStb.sol

74:         for (uint8 i = 0; i < users.length; i++) {

83:         for (uint8 i = 0; i < users.length; i++) {

92:         for (uint8 i = 0; i < users.length; i++) {

101:         for (uint8 i = 0; i < users.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

175:         for (uint128 j = 0; j < _custodians.length; ) {

186:         for (uint128 k = 0; k < _tokenConfig.length; ) {

510:         uint128 totalRatio = 0;

517:         for (uint128 i = 0; i < route.addresses.length; ) {

610:         uint128 totalTransferred = 0;

611:         for (uint128 i = 0; i < addresses.length; ) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)


## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | `decimals()` is not a part of the ERC-20 standard | 1 |
| [L-2](#L-2) | Division by zero not prevented | 2 |
| [L-3](#L-3) | `domainSeparator()` isn't protected against replay attacks in case of a future chain split  | 7 |
| [L-4](#L-4) | External call recipient may consume all transaction gas | 2 |
| [L-5](#L-5) | Initializers could be front-run | 4 |
| [L-6](#L-6) | Loss of precision | 1 |
| [L-7](#L-7) | Sweeping may break accounting if tokens with multiple addresses are used | 1 |
| [L-8](#L-8) | Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions | 13 |
| [L-9](#L-9) | Upgradeable contract not initialized | 18 |
### <a name="L-1"></a>[L-1] `decimals()` is not a part of the ERC-20 standard
The `decimals()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*Instances (1)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

688:         uint8 decimals = IERC20Metadata(token).decimals();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="L-2"></a>[L-2] Division by zero not prevented
The divisions below take an input parameter which does not have any zero-value checks, which may lead to the functions reverting when zero is passed.

*Instances (2)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

562:             : collateralAmount / scale;

568:         uint128 differenceInBps = (difference * STABLES_RATIO_MULTIPLIER) / ustbAmount;

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="L-3"></a>[L-3] `domainSeparator()` isn't protected against replay attacks in case of a future chain split 
Severity: Low.
Description: See <https://eips.ethereum.org/EIPS/eip-2612#security-considerations>.
Remediation: Consider using the [implementation](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/cryptography/EIP712.sol#L77-L90) from OpenZeppelin, which recalculates the domain separator if the current `block.chainid` is not the cached chain ID.
Past occurrences of this issue:
- [Reality Cards Contest](https://github.com/code-423n4/2021-06-realitycards-findings/issues/166)
- [Swivel Contest](https://github.com/code-423n4/2021-09-swivel-findings/issues/98)
- [Malt Finance Contest](https://github.com/code-423n4/2021-11-malt-findings/issues/349)

*Instances (7)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

85:     bytes32 private immutable _domainSeparator;

201:         _domainSeparator = _computeDomainSeparator();

430:     function getDomainSeparator() public view returns (bytes32) {

432:             return _domainSeparator;

434:         return _computeDomainSeparator();

439:         return ECDSA.toTypedDataHash(getDomainSeparator(), keccak256(encodeOrder(order)));

664:     function _computeDomainSeparator() internal view returns (bytes32) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="L-4"></a>[L-4] External call recipient may consume all transaction gas
There is no limit specified on the amount of gas used, so the recipient can use up all of the transaction's gas, causing it to revert. Use `addr.call{gas: <amount>}("")` or [this](https://github.com/nomad-xyz/ExcessivelySafeCall) library instead.

*Instances (2)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

339:             (bool success, ) = wallet.call{value: amount}("");

591:             (bool success, ) = (beneficiary).call{value: amount}("");

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="L-5"></a>[L-5] Initializers could be front-run
Initializers could be front-run, allowing an attacker to either set their own values, take ownership of the contract, and in the best case forcing a re-deployment

*Instances (4)*:
```solidity
File: ./contracts/ustb/UStb.sol

48:     function initialize(address admin, address minterContract) public initializer {

49:         __ERC20_init("UStb", "UStb");

50:         __ERC20Permit_init("UStb");

51:         __ReentrancyGuard_init();

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

### <a name="L-6"></a>[L-6] Loss of precision
Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator

*Instances (1)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

612:             uint128 amountToTransfer = (amount * ratios[i]) / ROUTE_REQUIRED_RATIO;

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="L-7"></a>[L-7] Sweeping may break accounting if tokens with multiple addresses are used
There have been [cases](https://blog.openzeppelin.com/compound-tusd-integration-issue-retrospective/) in the past where a token mistakenly had two addresses that could control its balance, and transfers using one address impacted the balance of the other. To protect against this potential scenario, sweep functions should ensure that the balance of the non-sweepable token does not change after the transfer of the swept tokens.

*Instances (1)*:
```solidity
File: ./contracts/ustb/UStb.sol

128:     function rescueTokens(

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

### <a name="L-8"></a>[L-8] Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions
See [this](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) link for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

*Instances (13)*:
```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

4: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

13: abstract contract SingleAdminAccessControlUpgradeable is IERC5313, ISingleAdminAccessControl, AccessControlUpgradeable {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20PermitUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20BurnableUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/utils/SafeERC20Upgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol";

8: import "../SingleAdminAccessControlUpgradeable.sol";

16:     ERC20BurnableUpgradeable,

17:     ERC20PermitUpgradeable,

19:     ReentrancyGuardUpgradeable,

20:     SingleAdminAccessControlUpgradeable

22:     using SafeERC20Upgradeable for IERC20Upgradeable;

133:         IERC20Upgradeable(token).safeTransfer(to, amount);

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

### <a name="L-9"></a>[L-9] Upgradeable contract not initialized
Upgradeable contracts are initialized via an initializer function rather than by a constructor. Leaving such a contract uninitialized may lead to it being taken over by a malicious user

*Instances (18)*:
```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

4: import "@openzeppelin/contracts-upgradeable/access/AccessControlUpgradeable.sol";

13: abstract contract SingleAdminAccessControlUpgradeable is IERC5313, ISingleAdminAccessControl, AccessControlUpgradeable {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

4: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20PermitUpgradeable.sol";

5: import "@openzeppelin/contracts-upgradeable/token/ERC20/extensions/ERC20BurnableUpgradeable.sol";

6: import "@openzeppelin/contracts-upgradeable/token/ERC20/utils/SafeERC20Upgradeable.sol";

7: import "@openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol";

8: import "../SingleAdminAccessControlUpgradeable.sol";

16:     ERC20BurnableUpgradeable,

17:     ERC20PermitUpgradeable,

19:     ReentrancyGuardUpgradeable,

20:     SingleAdminAccessControlUpgradeable

22:     using SafeERC20Upgradeable for IERC20Upgradeable;

39:         _disableInitializers();

48:     function initialize(address admin, address minterContract) public initializer {

49:         __ERC20_init("UStb", "UStb");

50:         __ERC20Permit_init("UStb");

51:         __ReentrancyGuard_init();

133:         IERC20Upgradeable(token).safeTransfer(to, amount);

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)


## Medium Issues


| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | `block.number` means different things on different L2s | 8 |
| [M-2](#M-2) | Centralization Risk for trusted owners | 39 |
### <a name="M-1"></a>[M-1] `block.number` means different things on different L2s
On Optimism, `block.number` is the L2 block number, but on Arbitrum, it's the L1 block number, and `ArbSys(address(100)).arbBlockNumber()` must be used. Furthermore, L2 block numbers often occur much more frequently than L1 block numbers (any may even occur on a per-transaction basis), so using block numbers for timing results in inconsistencies, especially when voting is involved across multiple chains. As of version 4.9, OpenZeppelin has [modified](https://blog.openzeppelin.com/introducing-openzeppelin-contracts-v4.9#governor) their governor code to use a clock rather than block numbers, to avoid these sorts of issues, but this still requires that the project [implement](https://docs.openzeppelin.com/contracts/4.x/governance#token_2) a [clock](https://eips.ethereum.org/EIPS/eip-6372) for each L2.

*Instances (8)*:
```solidity
File: ./contracts/ustb/UStbMinting.sol

116:         if (totalPerBlockPerAsset[block.number][asset].mintedPerBlock + mintAmount > _config.maxMintPerBlock) {

128:         if (totalPerBlockPerAsset[block.number][asset].redeemedPerBlock + redeemAmount > _config.maxRedeemPerBlock) {

138:         uint128 totalMintedThisBlock = totalPerBlock[uint128(block.number)].mintedPerBlock;

148:         uint128 totalRedeemedThisBlock = totalPerBlock[block.number].redeemedPerBlock;

235:         totalPerBlockPerAsset[block.number][order.collateral_asset].mintedPerBlock += order.ustb_amount;

236:         totalPerBlock[block.number].mintedPerBlock += order.ustb_amount;

276:         totalPerBlockPerAsset[block.number][order.collateral_asset].redeemedPerBlock += order.ustb_amount;

277:         totalPerBlock[block.number].redeemedPerBlock += order.ustb_amount;

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

### <a name="M-2"></a>[M-2] Centralization Risk for trusted owners

#### Impact:
Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

*Instances (39)*:
```solidity
File: ./contracts/SingleAdminAccessControl.sol

13: abstract contract SingleAdminAccessControl is IERC5313, ISingleAdminAccessControl, AccessControl {

25:     function transferAdmin(address newAdmin) external onlyRole(DEFAULT_ADMIN_ROLE) {

41:     function grantRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

50:     function revokeRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControl.sol)

```solidity
File: ./contracts/SingleAdminAccessControlUpgradeable.sol

13: abstract contract SingleAdminAccessControlUpgradeable is IERC5313, ISingleAdminAccessControl, AccessControlUpgradeable {

25:     function transferAdmin(address newAdmin) external onlyRole(DEFAULT_ADMIN_ROLE) {

41:     function grantRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

50:     function revokeRole(bytes32 role, address account) public override onlyRole(DEFAULT_ADMIN_ROLE) notAdmin(role) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/SingleAdminAccessControlUpgradeable.sol)

```solidity
File: ./contracts/ustb/UStb.sol

59:     function addMinter(address minterContract) external onlyRole(DEFAULT_ADMIN_ROLE) {

64:     function removeMinter(address minterContract) external onlyRole(DEFAULT_ADMIN_ROLE) {

73:     function addBlacklistAddress(address[] calldata users) external onlyRole(BLACKLIST_MANAGER_ROLE) {

82:     function removeBlacklistAddress(address[] calldata users) external onlyRole(BLACKLIST_MANAGER_ROLE) {

91:     function addWhitelistAddress(address[] calldata users) external onlyRole(WHITELIST_MANAGER_ROLE) {

100:     function removeWhitelistAddress(address[] calldata users) external onlyRole(WHITELIST_MANAGER_ROLE) {

111:     function redistributeLockedAmount(address from, address to) external nonReentrant onlyRole(DEFAULT_ADMIN_ROLE) {

132:     ) external nonReentrant onlyRole(DEFAULT_ADMIN_ROLE) {

143:     function mint(address to, uint256 amount) external onlyRole(MINTER_CONTRACT) {

159:     function updateTransferState(TransferState code) external onlyRole(DEFAULT_ADMIN_ROLE) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStb.sol)

```solidity
File: ./contracts/ustb/UStbMinting.sol

21: contract UStbMinting is IUStbMinting, SingleAdminAccessControl, ReentrancyGuard {

226:         onlyRole(MINTER_ROLE)

268:         onlyRole(REDEEMER_ROLE)

292:     function setGlobalMaxMintPerBlock(uint128 _globalMaxMintPerBlock) external onlyRole(DEFAULT_ADMIN_ROLE) {

298:     function setGlobalMaxRedeemPerBlock(uint128 _globalMaxRedeemPerBlock) external onlyRole(DEFAULT_ADMIN_ROLE) {

304:     function disableMintRedeem() external onlyRole(GATEKEEPER_ROLE) {

336:     ) external nonReentrant onlyRole(COLLATERAL_MANAGER_ROLE) {

348:     function removeSupportedAsset(address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

360:     function removeCustodianAddress(address custodian) external onlyRole(DEFAULT_ADMIN_ROLE) {

367:     function removeMinterRole(address minter) external onlyRole(GATEKEEPER_ROLE) {

373:     function removeRedeemerRole(address redeemer) external onlyRole(GATEKEEPER_ROLE) {

379:     function removeCollateralManagerRole(address collateralManager) external onlyRole(GATEKEEPER_ROLE) {

384:     function removeWhitelistedBenefactor(address benefactor) external onlyRole(DEFAULT_ADMIN_ROLE) {

392:     function addCustodianAddress(address custodian) public onlyRole(DEFAULT_ADMIN_ROLE) {

400:     function addWhitelistedBenefactor(address benefactor) public onlyRole(DEFAULT_ADMIN_ROLE) {

633:     function addSupportedAsset(address asset, TokenConfig memory _tokenConfig) external onlyRole(DEFAULT_ADMIN_ROLE) {

641:     function setMaxMintPerBlock(uint128 _maxMintPerBlock, address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

651:     function setMaxRedeemPerBlock(uint128 _maxRedeemPerBlock, address asset) external onlyRole(DEFAULT_ADMIN_ROLE) {

669:     function setTokenType(address asset, TokenType tokenType) external onlyRole(DEFAULT_ADMIN_ROLE) {

676:     function setStablesDeltaLimit(uint128 _stablesDeltaLimit) external onlyRole(DEFAULT_ADMIN_ROLE) {

681:     function setUStbToken(IUStb _ustb) external onlyRole(DEFAULT_ADMIN_ROLE) {

```
[Link to code](https://github.com/code-423n4/2024-11-ethena-labs/blob/main/./contracts/ustb/UStbMinting.sol)

