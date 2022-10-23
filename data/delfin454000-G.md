### Should use non-strict inequality if appropriate
Non-strict inequalities are cheaper than strict ones. In the case below, the comment line refers to "greater than or equal to", suggesting that use of a nonstrict inequality would be more accurate as well as being cheaper
___
[JBTiered721DelegateStore.sol: L216](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L663-L665)
```solidity
      // Make sure the tier's contribution floor is greater than or equal to the previous contribution floor.
      if (_i != 0 && _tierToAdd.contributionFloor < _tiersToAdd[_i - 1].contributionFloor)
        revert INVALID_PRICE_SORT_ORDER();
```
___
___

### Inline a function that’s only used once
It costs less gas to inline the code of functions that are only called once. Doing so saves the cost of allocating the function selector, and the cost of the jump when the function is called. Below are two examples.
___
[JB721Delegate.sol: L105-113](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L105-L113)
```solidity
  function redeemParams(JBRedeemParamsData calldata _data)
    external
    view
    override
    returns (
      uint256 reclaimAmount,
      string memory memo,
      JBRedemptionDelegateAllocation[] memory delegateAllocations
    )
```
___
[JBTiered721FundingCycleMetadataResolver.sol: L15-17](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol#L15-L17)
```solidity
  function changingPricingResolverPaused(uint256 _data) internal pure returns (bool) {
    return ((_data >> 2) & 1) == 1;
  }
```
___
___

### Avoid using `booleans` for storage
Using a `bool` for storage incurs more overhead than using `uint256` or any type that takes up a full word. Below are three examples
___
[JBTiered721Delegate.sol: L543](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L543)
```solidity
    bool _expectMintFromExtraFunds;
```
___
[JBTiered721Delegate.sol: L546](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L546)
```solidity
    bool _dontOverspend;
```
___
[JBTiered721Delegate.sol: L555](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L555)
```solidity
      bool _dontMint;
```
___
___


### Avoid storage of uints or ints smaller than 32 bytes, if possible
Storage of uints or ints smaller than 32 bytes incurs overhead. Instead, use a size of at least 32 for `digitlength` below, then downcast where needed:

[JBIpfsDecoder.sol: L48](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L48)
```solidity
    uint8 digitlength = 1;
```
___
___


### Array length should not be looked up during every iteration of a `for` loop
Since calculating the array length costs gas, it would be better to read the length of the array from memory before executing the loop for the three loops referenced below:

[JBIpfsDecoder.sol: L49-64](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49-L64)

[JBIpfsDecoder.sol: L76-78](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76-L78)

[JBIpfsDecoder.sol: L84-86](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84-L86)
___
___

### Use `++i` instead of `i++` to increase count in a `for` loop
Since use of  `i++` (or equivalent counter) costs more gas, use `++i` in the loops referenced below: 

[JBIpfsDecoder.sol: L68-70](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68-L70)

[JBIpfsDecoder.sol: L76-78](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76-L78)

[JBIpfsDecoder.sol: L84-86](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84-L86)
___
___

### Avoid use of default "checked" behavior in a `for` loop
Underflow/overflow checks are made every time `++i` (or `i++` or equivalent counter) is called. Such checks are unnecessary since `i` is already limited. Therefore, use `unchecked {++i}`/`unchecked {i++}` instead, as shown below:

[JBIpfsDecoder.sol: L68-70](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68-L70)
```solidity
    for (uint256 i = 0; i < _length; i++) {
      output[i] = _array[i];
    }
```
Suggestion:
```solidity
    for (uint256 i = 0; i < _length;) {
      output[i] = _array[i];

      unchecked {
        ++i;
      }
    }
```
___

Similarly for the three `for` loops referenced below:

[JBIpfsDecoder.sol: L49-64](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49-L64)

[JBIpfsDecoder.sol: L76-78](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76-L78)

[JBIpfsDecoder.sol: L84-86](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84-L86)
___
___