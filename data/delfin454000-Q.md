## QA Report low

### Missing checks for address(0x0) when assigning values to address state variables
Checks are missing for all three variables below:

[JBTiered721DelegateDeployer.sol: L51-53](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L51-L53)
```solidity
    globalGovernance = _globalGovernance;
    tieredGovernance = _tieredGovernance;
    noGovernance = _noGovernance;
```
___
___


## QA Report: non-critical

### Typos
___
[JBTiered721DelegateDeployer.sol: L132](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L132)
```solidity
      // Copy the bytecode (our initialise part is 13 bytes long)
```
Change `initialise` to `initialize`. The spelling of a given word should be consistent. Most of the contest uses the American English spelling of `initialize`. For example, [JBTiered721DelegateDeployer.sol: L84](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L84). The following six additional instances of `initialise` also should be corrected:

[JBTiered721DelegateStore.sol: L233](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L233)

[JBTiered721DelegateStore.sol: L716](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L716)

[JBTiered721DelegateStore.sol: L947](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L947)

[JBTiered721DelegateStore.sol: L1032](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1032)

[JBTiered721DelegateStore.sol: L1183](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1183)

[JBBitmap.sol: L13](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L13)
___
[JBTiered721DelegateProjectDeployer.sol: L19](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L19)
```solidity
  JBOperatable: Several functions in this contract can only be accessed by a project owner, or an address that has been preconfifigured to be an operator of the project.
```
Change `preconfifigured` to `preconfigured`
___
[JBTiered721DelegateProjectDeployer.sol: L34](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L34)
```solidity
    The contract responsibile for deploying the delegate. 
```
Change `responsibile` to `responsible`
___
[JBTiered721Delegate.sol: L17](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L17)
```solidity
  Delegate that offers project contributors NFTs with tiered price floors upon payment and the ability to redeem NFTs for treasury assets based based on price floor.
```
Remove repeated word `based`
___
The same typo (`accross`) occurs in both lines below:

[JBTiered721Delegate.sol: L121](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L121)

[JBTiered721DelegateStore.sol: L497](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L497)
```solidity
    @return balance The number of tokens owners by the owner accross all tiers.
```
Change `accross` to `across` in both cases
___
The same typo (`adherance`) occurs in both lines below:

[JBTiered721Delegate.sol: L173](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L173)

[JB721Delegate.sol: L176](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L176)
```solidity
    @param _interfaceId The ID of the interface to check for adherance to.
```
Change `adherance` to `adherence` in both cases
___
[JBTiered721Delegate.sol: L268](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L268)
```solidity
    // Keep a reference to the number of tiers there are to mint reserved for.
```
Change `reserved` to `reserves`
___
The same typo (`beneificiary`) occurs in both lines below:

[JBTiered721Delegate.sol: L363](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L363)

[JBTiered721Delegate.sol: L368](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L368)
```solidity
    Sets the beneificiary of the reserved tokens for tiers where a specific beneficiary isn't set. 
```
Change `beneificiary` to `beneficiary` in both cases
___
[JBTiered721Delegate.sol: L545](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L545)
```solidity
    // Keep a reference to the flag indicating if the transaction should revert if all provded funds aren't spent. Defaults to false, meaning only a minimum payment is enforced.
```
Change `provded` to `provided`
___
[JBTiered721Delegate.sol: L557](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L557)
```solidity
      // Keep a reference to the the specific tier IDs to mint.
```
Remove repeated word `the`
___
[JBTiered721Delegate.sol: L717](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L717)
```solidity
    User the hook to register the first owner if it's not yet regitered.
```
Change `User` to `Use` and `regitered` to `registered`
___
[JBTiered721DelegateStore.sol: L176](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L176)
```solidity
    Custom token URI resolver, superceeds base URI.
```
Change `superceeds` to `supersedes`
___
[JBTiered721DelegateStore.sol: L230](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L230)
```solidity
    // Keep a referecen to the tier being iterated on.
```
Change `referecen` to `reference`
___
[JBTiered721DelegateStore.sol: L597](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L597)
```solidity
    @return The reserved token benficiary.
```
Change `benficiary` to `beneficiary`
___
[JBTiered721DelegateStore.sol: L719](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L719)
```solidity
        // Keep a reference to the idex to iterate on next.
```
Change `idex` to `index`
___
[JBTiered721DelegateStore.sol: L832](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L832)
```solidity
    // Keep a reference to the number of burned in the tier.
```
Change `of burned` to `burned`
___
[JBTiered721DelegateStore.sol: L852](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L852)
```solidity
    @param _beneficiary The reservd token beneficiary.
```
Change `reservd` to `reserved`
___
[JB721Delegate.sol: L88](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L88)
```solidity
    // Forward the recieved weight and memo, and use this contract as a pay delegate.
```
Change `recieved` to `received`
___
[JB721Delegate.sol: L144](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L144)
```solidity
    // These conditions are all part of the same curve. Edge conditions are separated because fewer operation are necessary.
```
Change `operation` to `operations`
___
[JBIpfsDecoder.sol: L42](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L42)
```solidity
    Written by Martin Ludfall - Licence: MIT
```
Change `Licence` to `License`
___
___

### NatSpec statements are missing
NatSpec statements are entirely missing for the two `constructors` referenced below:

[JBTiered721DelegateDeployer.sol: L46-54](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L46-L54)
```solidity
  constructor(
    JB721GlobalGovernance _globalGovernance,
    JB721TieredGovernance _tieredGovernance,
    JBTiered721Delegate _noGovernance
  ) {
    globalGovernance = _globalGovernance;
    tieredGovernance = _tieredGovernance;
    noGovernance = _noGovernance;
  }
```
___
[JBTiered721Delegate.sol: L185-187](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L185-L187)
```solidity
  constructor() {
    codeOrigin = address(this);
  }
```
___
NatSpec statements are also entirely missing for the four `functions` below:

[JBTiered721DelegateDeployer.sol: L115-118](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L115-L118)
```solidity
  function _clone(address _targetAddress) internal returns (address _out) {
    assembly {
      // Get deployed/runtime code size
      let _codeSize := extcodesize(_targetAddress)
```
___
[JBTiered721FundingCycleMetadataResolver.sol: L6-9](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol#L6-L9)
```solidity
library JBTiered721FundingCycleMetadataResolver {
  function transfersPaused(uint256 _data) internal pure returns (bool) {
    return (_data & 1) == 1;
  }
```
___
[JBTiered721FundingCycleMetadataResolver.sol: L11-13](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol#L11-L13)
```solidity
  function mintingReservesPaused(uint256 _data) internal pure returns (bool) {
    return ((_data >> 1) & 1) == 1;
  }
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

### NatSpec is incomplete
The following `functions` are missing `@return`:

[JB721TieredGovernance.sol: L68-81](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L68-L81)

[JB721TieredGovernance.sol: L84-92](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L84-L92)

[JB721TieredGovernance.sol: L95-108](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L95-L108)

[JB721TieredGovernance.sol: L111-118](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L111-L118)

[JB721TieredGovernance.sol: L121-135](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L121-L135)

[JBTiered721Delegate.sol: L167-179](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L167-L179)

[JB721Delegate.sol: L170-191](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L170-L191)
___
___


### Long single-line comments 
In theory, comments over 80 characters should wrap using multi-line comment syntax. Even if somewhat longer comments (up to 120 characters) are acceptable and a scroll bar is provided, there are cases where very long comments interfere with readability. Below are five instances of extra-long comments whose readability could be improved by wrapping, as shown:
___
[JBTiered721DelegateDeployer.sol: L15](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L15)
```solidity
  IJBTiered721DelegateDeployer: General interface for the generic controller methods in this contract that interacts with funding cycles and tokens according to the protocol's rules.
```
Suggestion:
```solidity
  IJBTiered721DelegateDeployer: General interface for the generic controller methods in this contract 
      that interacts with funding cycles and tokens according to the protocol's rules.
```
Similarly for the following equivalent comment: [JBTiered721DelegateProjectDeployer.sol: L15](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L15)
___
[JB721TieredGovernance.sol: L235](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L235)
```solidity
    Transfers, mints, or burns tier voting units. To register a mint, `from` should be zero. To register a burn, `to` should be zero. Total supply of voting units will be adjusted with mints and burns.
```
Suggestion:
```solidity
    Transfers, mints, or burns tier voting units. To register a mint, `from` should be zero. 
        To register a burn, `to` should be zero. Total supply of voting units will be adjusted with mints and burns.
```
___
[JBTiered721Delegate.sol: L198](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L198)
```solidity
    @param _pricing The tier pricing according to which token distribution will be made. Must be passed in order of contribution floor, with implied increasing value.
```
Suggestion:
```solidity
    @param _pricing The tier pricing according to which token distribution will be made. 
        Must be passed in order of contribution floor, with implied increasing value.
```
___
[JBTiered721Delegate.sol: L545](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L545)
```solidity
    // Keep a reference to the flag indicating if the transaction should revert if all provded funds aren't spent. Defaults to false, meaning only a minimum payment is enforced.
```
Suggestion:
```solidity
    // Keep a reference to the flag indicating if the transaction should revert if all provided funds aren't spent. 
    //    Defaults to false, meaning only a minimum payment is enforced.
```
___
[JB721Delegate.sol: L144](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L144)
```solidity
    // These conditions are all part of the same curve. Edge conditions are separated because fewer operation are necessary.
```
Suggestion:
```solidity
    // These conditions are all part of the same curve. Edge conditions
    //    are separated because fewer operation are necessary.
```
___
___

### Open items
Comments that contain language referring to possible open items should be addressed and modified or removed
___
[JBTiered721DelegateProjectDeployer.sol: L75](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L75)
```solidity
    // Get the project ID, optimistically knowing it will be one greater than the current count.
```
While it might just be an example of awkward language, the phrase "optimistically knowing" appears ambiguous and should be reviewed and corrected
___
___

### Use scientific notation for large multiples of 10 rather than decimal literals
For readability, used scientific notation (e.g. 1e6) rather than decimal literals (e.g. 1000000) for large multiples of ten
___
[JBTiered721DelegateDeployer.sol: L123-124](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L123-L124)
```solidity
      // Shift the length to the length placeholder, in the constructor
      let _mask := mul(_codeSize, 0x100000000000000000000000000000000000000000000000000000000)
```
___
___


### Missing `require` revert string
An error message should be included to enable users to understand the reason for failure.

Recommendation: Add a brief (<= 32 char) explanatory message to both of the `requires` referenced below. Or use custom error codes (which would be cheaper than revert strings).

___
[JBTiered721Delegate.sol: L216](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L216)
```solidity
    require(address(this) != codeOrigin);
```
___
[JBTiered721Delegate.sol: L218](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L218)
```solidity
    require(address(store) == address(0));
```
___
___

### Use consistent initialization of counters in `for` loops 
Most `for` loop counters in `Juicebox` are not initiated to zero whereas a few are. It is not necessary to initialize `for` loop counters to zero since this is their default value. 

For consistency, it makes sense to omit counter initializations in the `for` loops below. 

___
[JBIpfsDecoder.sol: L49-62](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49-L62)
```solidity
    for (uint256 i = 0; i < _source.length; ++i) {
      uint256 carry = uint8(_source[i]);
      for (uint256 j = 0; j < digitlength; ++j) {
        carry += uint256(digits[j]) * 256;
        digits[j] = uint8(carry % 58);
        carry = carry / 58;
      }

      while (carry > 0) {
        digits[digitlength] = uint8(carry % 58);
        digitlength++;
        carry = carry / 58;
      }
    }
```
___
[JBIpfsDecoder.sol: L68-70](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68-L70)
```solidity
    for (uint256 i = 0; i < _length; i++) {
      output[i] = _array[i];
    }
```
___
[JBIpfsDecoder.sol: L76-78](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76-L78)
```solidity
    for (uint256 i = 0; i < _input.length; i++) {
      output[i] = _input[_input.length - 1 - i];
    }
```
___
[JBIpfsDecoder.sol: L84-86](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84-L86)
```solidity
    for (uint256 i = 0; i < _indices.length; i++) {
      output[i] = _ALPHABET[_indices[i]];
    }
```
___
___