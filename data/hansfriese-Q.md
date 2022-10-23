### [L-01] Wrong events
- https://github.com/jbx-protocol/juice-nft-rewards/blob/7d7aec6f8642c6e1c5e55cfed67eb69b6fc8174a/contracts/JB721TieredGovernance.sol#L287

```solidity
emit TierDelegateVotesChanged(_from, _oldValue, _newValue, _tierId, msg.sender);
```

It should be modified like below.

```solidity
emit TierDelegateVotesChanged(_from, _tierId, _oldValue, _newValue, msg.sender);
```

### [L-02] Incorrect pre-check condition
- https://github.com/jbx-protocol/juice-nft-rewards/blob/7d7aec6f8642c6e1c5e55cfed67eb69b6fc8174a/contracts/JBTiered721Delegate.sol#L551

```solidity
    if (
      _data.metadata.length > 36 && 
      bytes4(_data.metadata[32:36]) == type(IJB721Delegate).interfaceId
    ) {
```

4 bytes of interfaceId is saved from 32 to 36(exclusive) and it's enough to check if `_data.metadata.length >= 36` instead of `_data.metadata.length > 36`.

### [L-03] Immutable addresses should be 0-checked
- https://github.com/jbx-protocol/juice-nft-rewards/blob/7d7aec6f8642c6e1c5e55cfed67eb69b6fc8174a/contracts/JBTiered721DelegateDeployer.sol#L51-L53
- https://github.com/jbx-protocol/juice-nft-rewards/blob/7d7aec6f8642c6e1c5e55cfed67eb69b6fc8174a/contracts/JBTiered721DelegateProjectDeployer.sol#L52-L53

### [N-01] require()/revert() statements should have descriptive reason strings
- https://github.com/jbx-protocol/juice-nft-rewards/blob/7d7aec6f8642c6e1c5e55cfed67eb69b6fc8174a/contracts/JBTiered721Delegate.sol#L215-L218

```solidity
    // Make the original un-initializable.
    require(address(this) != codeOrigin);
    // Stop re-initialization.
    require(address(store) == address(0));
```