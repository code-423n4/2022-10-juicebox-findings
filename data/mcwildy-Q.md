# Qa Report

## 2022-10-JUICEBOX

### Do not explicitly `return` from a function if it already has named declared return variables

[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol):

```solidity
line#L631: return leftoverAmount;
```

### Contracts should use a fixed compiler version to avoid potential bugs

[JBTiered721DelegateStore.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol):

```solidity
line#L2:   pragma solidity ^0.8.16;
```

[JB721TieredGovernance.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol):

```solidity
line#L2:   pragma solidity ^0.8.16;
```

[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol):

```solidity
line#L2:   pragma solidity ^0.8.16;
```

[JB721GlobalGovernance.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721GlobalGovernance.sol):

```solidity
line#L2:   pragma solidity ^0.8.16;
```

[JB721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol):

```solidity
line#L2:   pragma solidity ^0.8.16;
```

[JBTiered721DelegateDeployer.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateDeployer.sol):

```solidity
line#L2:   pragma solidity ^0.8.16;
```

[JBTiered721DelegateProjectDeployer.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol):

```solidity
line#L2:   pragma solidity ^0.8.16;
```

### Missing natspec comments

https://docs.soliditylang.org/en/develop/natspec-format.html

[JBTiered721FundingCycleMetadataResolver.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol):

```solidity
line#L1:   // SPDX-License-Identifier: MIT
```

### Events should be emmitted on every critical state changes

Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol):

```solidity
line#L202: function initialize(
```

[JB721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol):

```solidity
line#L203: function _initialize(
```

### Use `external` visibility modifier for function that are not being invoked by the contract

[JB721TieredGovernance.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol):

```solidity
line#L147: function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)

line#L177: function setTierDelegate(address _delegatee, uint256 _tierId) public virtual override {
```

[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol):

```solidity
line#L138: function tokenURI(uint256 _tokenId) public view override returns (string memory) {
```

[JBTiered721DelegateStore.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol):

```solidity
line#L499: function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) {

line#L523: function redemptionWeightOf(address _nft, uint256[] calldata _tokenIds)

line#L585: function tierIdOfToken(uint256 _tokenId) public pure override returns (uint256) {

line#L599: function reservedTokenBeneficiaryOf(address _nft, uint256 _tierId)
```