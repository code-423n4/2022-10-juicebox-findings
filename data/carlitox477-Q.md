# \[LOW\] JB721Delegate#initialize _fundingCycleStore lack of zero address check can lead to redeployment

## Impact
initialize function does not check that ```_fundingCycleStore``` is not zero. Given that state variable ```fundingCycleStore``` can not be set anywhere else, setting it to zero can lead to contract redeployment

## POC
The deployer mistakenly call JB721Delegate#initialize with ```_fundingCycleStore = IJBFundingCycleStore(0)``` as parameter, then ```mintReservesFor``` and ```_beforeTokenTransfer``` will always revert

## Tools
Manual review.

## Mitigation steps
Add a require statement ```require(_fundingCycleStore != IJBFundingCycleStore(0))``` in JB721Delegate#initialize function



# \[LOW\] Lack of zero-address check in LB721Delegate#_initialize function
Checking addresses against zero-address during initialization or during setting is a security best-practice. However, such checks are missing in address variable initializations/changes.

## Impact
Allowing zero-addresses will lead to contract reverts and force redeployments if there are no setters for such address variables.

[Affected code](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L203-L213)

Setting zero address will lead to redeployment, and payable functions [didPay](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L228) and [didRedeem](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L249) will be unusable.

## Tools used
Manual review

## Mitigation steps
Add zero address check.

# \[Non-critical\] JB721TierdGovernance#delegateTier can emit wrong DelegateChanged event.
The function does not check that ```_oldDelegate != _delegatee```, emiting a wrong ```DelegateChanged``` event.

# \[Non-critical\] TierDelegateVotesChanged wrong parameter name
While ```TierDelegateVotesChanged``` last parameter should changed from ```callre``` to ```caller```.

# \[Non-critical\] JB721TieredGovernance#_moveTierDelegateVotes Wrong event emission

While ```TierDelegateVotesChanged``` is defined as
```solidity
event TierDelegateVotesChanged(
    address indexed delegate,
    uint256 indexed tierId,
    uint256 previousBalance,
    uint256 newBalance,
    address callre
);
```

[This line](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L287) set parameters wrong. it should be changed to:
```solidity
emit TierDelegateVotesChanged(_from, _tierId, _oldValue, _newValue, , msg.sender);
```

# JBTiered721DelegateDeployer#constructor and JBTiered721DelegateProjectDeployer#constructor lack of address zero check
[JBTiered721DelegateDeployer#constructor](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L46-L54)
[JBTiered721DelegateProjectDeployer#constructor](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L47-L54)

Checking addresses against zero-address during initialization or during setting is a security best-practice. However, such checks are missing in address variable initializations/changes