## 1. Explicitly assingning default values to variables is a waste of gas
### Use `uint256 i;` instead of `uint256 i = 0;`

_contracts/libraries/JBIpfsDecoder.sol:_ [L49](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L49)
[L68](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L68)
[L76](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L76)
[L84](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L84)

## 2. Cache storage variables in function call stack to save gas

_contracts/JBTiered721Delegate.sol:_ [L143-L152](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L143-L152)

## 3. Prefix incrementing and decrementing costs around 6 gas less than the postfix ones
### e.g. ++var is cheaper than var++

_contracts/libraries/JBIpfsDecoder.sol:_ [L68](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L68)
[L76](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L76)
[L84](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L84)

_contracts/JBTiered721DelegateStore.sol:_ [L1108](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol#L1108)

## 4. use x = x + y instead of x+= y

_contracts/JBTiered721DelegateStore.sol:_ [L827](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol#L827)

## 5. Calldata is cheaper than memory for function input

_contracts/abstract/JB721Delegate.sol:_ [L206](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol#L206)
[L311](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol#L311)
[L323](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol#L323)

_contracts/libraries/JBIpfsDecoder.sol:_ [L22](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L22)
[L44](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L44)
[L74](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L74)
[L82](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L82)

_contracts/libraries/JBBitmap.sol:_ [L29](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBBitmap.sol#L29)
[L59](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBBitmap.sol#L59)

_contracts/JBTiered721DelegateDeployer.sol:_ [L71](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateDeployer.sol#L71)

_contracts/JBTiered721DelegateStore.sol:_ [L628](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol#L628)
[L1091](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol#L1091)
[L1227](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol#L1227)

_contracts/JBTiered721Delegate.sol:_ [L205](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L205)
[L208](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L208)
[L210](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L210)
[L211](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L211)
[L264](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L264)
[L290](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L290)
[L480](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L480)
[L598](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L598)
[L652](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L652)
[L789](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L789)

_contracts/JB721TieredGovernance.sol:_ [L147](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol#L147)
[L313](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol#L313)

_contracts/JB721GlobalGovernance.sol:_ [L55](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721GlobalGovernance.sol#L55)

_contracts/JBTiered721DelegateProjectDeployer.sol:_ [L72](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol#L72)
[L73](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol#L73)
[L109](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol#L109)
[L110](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol#L110)
[L152](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol#L152)
[L191](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol#L191)
[L218](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol#L218)

## 6. Use `x < y + 1` in stead of `x <= y`

_contracts/JB721TieredGovernance.sol:_ [L133](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol#L133)

_contracts/JBTiered721DelegateStore.sol:_ [L903](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol#L903)

## 7. When comparing variables of type `uint`, use `require(x != 0)` instead of `require(x > 0)`

_contracts/libraries/JBIpfsDecoder.sol:_ [L57](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L57)

_contracts/JBTiered721DelegateStore.sol:_ [L1254](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol#L1254)

## 8. Use `constant` and `immutable` for constants

_contracts/JBTiered721Delegate.sol:_ [L48](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L48)

## 9. Consider marking functions as `payable` if there is no risk of sending value through them
### This change will save gas each time a function is called

_contracts/JBTiered721Delegate.sol:_ [L370](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L370)
[L402](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L402)
[L418](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L418)

## 10. Place `i++` in an `unchecked` blocks in for-loops

_contracts/libraries/JBIpfsDecoder.sol:_ [L49](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L49)
[L51](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L51)
[L68](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L68)
[L76](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol#L76)

_contracts/JBTiered721Delegate.sol:_ [L341](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L341)
[L355](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol#L355)

