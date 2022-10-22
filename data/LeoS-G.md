## [G-01] Using `calldata` instead of `memory` for read only argument in external function
If a function parameter is read only, it is cheaper in gas to use `calldata` instead of `memory`.

6 instances:

 - https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L264
 - https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290
 - https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L386
 - https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L72
 - https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L109
 - https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L152

Consider changing `memory` to `calldata` in these lines

With those change, these evolutions in gas average report can be observed:

    (The following changes are not always explained.
    Some of them are due to too much variance or are just the effect
    of better optimizations of other functions.
    So we have to be critical about them.)
    
    JB721GlobalGovernance: Deployment: 4152378 -> 4132553 (-19825)
    JB721GlobalGovernance: didPay: 179733 -> 168252 (-11481)
    JB721TieredGovernance: Deployment: 4280949 -> 4264732 (-16217)
    JB721TieredGovernance: didPay: 180673 ->  169192 (-11481)
    JB721TieredGovernance: getTierVotes: 1447 -> 1180 (-267)
    JB721TieredGovernance: setTierDelegate: 29792 -> 29503 (-289)
    JBTiered721Delegate: Deployment:  3701538 -> 3681713 (-19825)
    JBTiered721Delegate: adjustTiers: 83233 -> 83298 (+65)
    JBTiered721Delegate: balanceOf: 16225 -> 16091 (-134)
    JBTiered721Delegate: didPay: 70891 -> 69972 (-919)
    JBTiered721Delegate: redeemParams: 19244 -> 19627 (+383)
    JBTiered721DelegateDeployer:  deployDelegateFor: 4827568 -> 4776781 (-50787)
    
    (The following modifications are purely the result of
    the changes made.)
    
    JBTiered721DelegateProjectDeployer: Deployment: 1668835 -> 1579737 (-89098)
    JBTiered721DelegateProjectDeployer: launchProjectFor: 5229396 -> 5202214 (-27182)