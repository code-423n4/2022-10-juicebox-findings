# JUICE-NFT-REWARDS
## Low Risk And Non-Critical Issues

### Adding a `return` statement when the function defines a named return variable, is redundant


_There are **1** instances of this issue:_

```solidity
File: contracts/JBTiered721Delegate.sol

613:  function _mintBestAvailableTier(
631:  return leftoverAmount;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

### Events not emmited on important state changes
Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

_There are **2** instances of this issue:_

```solidity
File: contracts/JBTiered721Delegate.sol

202:  function initialize(
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

```solidity
File: contracts/abstract/JB721Delegate.sol

203:  function _initialize(
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol

### `public` functions not called by the contract should be declared `external` instead


_There are **8** instances of this issue:_

```solidity
File: contracts/JB721TieredGovernance.sol

147:  function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)

177:  function setTierDelegate(address _delegatee, uint256 _tierId) public virtual override {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol

```solidity
File: contracts/JBTiered721Delegate.sol

138:  function tokenURI(uint256 _tokenId) public view override returns (string memory) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

```solidity
File: contracts/JBTiered721DelegateStore.sol

499:  function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) {

523:  function redemptionWeightOf(address _nft, uint256[] calldata _tokenIds)

550:  function totalRedemptionWeight(address _nft) public view override returns (uint256 weight) {

585:  function tierIdOfToken(uint256 _tokenId) public pure override returns (uint256) {

599:  function reservedTokenBeneficiaryOf(address _nft, uint256 _tierId)
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol

### Use `1e18` instead of `10**18` or `1000000000000000000`


_There are **2** instances of this issue:_

```solidity
File: contracts/JBTiered721Delegate.sol

531:  10**pricingDecimals,
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

```solidity
File: contracts/JBTiered721DelegateDeployer.sol

124:  let _mask := mul(_codeSize, 0x100000000000000000000000000000000000000000000000000000000)
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateDeployer.sol

### Non-library/interface files should use fixed compiler versions, not floating ones


_There are **7** instances of this issue:_

```solidity
File: contracts/JB721GlobalGovernance.sol

2:    pragma solidity ^0.8.16;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721GlobalGovernance.sol

```solidity
File: contracts/JB721TieredGovernance.sol

2:    pragma solidity ^0.8.16;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol

```solidity
File: contracts/JBTiered721Delegate.sol

2:    pragma solidity ^0.8.16;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

```solidity
File: contracts/JBTiered721DelegateDeployer.sol

2:    pragma solidity ^0.8.16;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateDeployer.sol

```solidity
File: contracts/JBTiered721DelegateProjectDeployer.sol

2:    pragma solidity ^0.8.16;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol

```solidity
File: contracts/JBTiered721DelegateStore.sol

2:    pragma solidity ^0.8.16;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol

```solidity
File: contracts/abstract/JB721Delegate.sol

2:    pragma solidity ^0.8.16;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol

### Natspec is missing


_There are **2** instances of this issue:_

```solidity
File: contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol

1:    // SPDX-License-Identifier: MIT
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol

