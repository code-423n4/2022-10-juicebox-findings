# Low


## Missing address(0) checks

On [JBTiered721Delegate.sol#L224](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L224) you should revert if `_store` is `address(0)`
On [JBTiered721DelegateProjectDeployer.sol#L52](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L52) you should revert if `_controller` is `address(0)`
On [JBTiered721DelegateProjectDeployer.sol#L53](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L53) you should revert if `_delegateDeployer` is `address(0)`
On [JBTiered721DelegateProjectDeployer.sol#L71](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1/contracts/JBTiered721DelegateProjectDeployer.sol#L71)  you should revert if `_owner` is `address(0)`


## Check for valid `pricingDecimals` threshold
On [JBTiered721Delegate.sol#L226](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L226) it should be a validation to check that price decimals are grater than 0 and less than 19 (between 1 and 18) that is consider as valid token decimals


## Avoid empty revert messages
On:
[JBTiered721Delegate.sol#L216](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L216)
[JBTiered721Delegate.sol#L218](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L218)


## Add checks to prevent invalid future block
On `getPastTierVotes` [JB721TieredGovernance.sol#L107](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L107) add a check like on [JB721TieredGovernance.sol#L133](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L133) to prevent invalid read of a future block