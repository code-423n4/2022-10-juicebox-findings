## Remove Shorthand Addition/Subtraction Assignment
Instead of using the shorthand of addition/subtraction assignment operators (```+=```, ```-=```)  
it costs less to remove the shorthand (```  x += y  ``` same as ```      x = x+y  ```)  saves ~22 gas
For example:
```
Line 506:  balance += tierBalanceOf[_nft][_owner][_i];
```
[Line 506](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L506)

could be refactored to

```
Line 506:  balance = balance + tierBalanceOf[_nft][_owner][_i];
```



## Public Functions Not Called by the Contract Should Be Declared External Instead
Functions not internally called may have their visibility changed to external to save gas.
For Example:



```
Line 214:  ) public override {
```
[Line 214](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L214)

## Usage of ```uint```/```int``` Smaller Than 32 bytes (256 Bits) Incurs Overhead
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed for example:

```
Line 66:  function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {;
```

[Line 66](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L66)

```
Line 48:    uint8 digitlength = 1;
```

[Line 48](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L48)



## Functions With Access Control Cheaper if Payable
A function with access control marked as payable will be cheaper for legitimate callers: the compiler removes checks for ```msg.value```, saving approximately ```20``` gas per function call.

For example:

```
Line 290   function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)
Line 291     external
Line 292     override
Line 293:    onlyOwner
```
[Line 293](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L293)


```
Line 321   function adjustTiers(JB721TierParams[] calldata _tiersToAdd, uint256[] calldata _tierIdsToRemove)
Line 322     external
Line 323     override
Line 324:    onlyOwner
```
[Line 324](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L324)



```
Line 370:  function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {
```
[Line 370](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L370)

## Uncheck arithmetics operations that can’t underflow/overflow
Solidity version 0.8+ comes with an implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible, some gas can be saved by using an unchecked block.
For example:

```
    for (uint256 i = 0; i < _input.length; i++) {
      output[i] = _input[_input.length - 1 - i];
    }
```
[Line 76-78](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76-L78)

could be refactored to as:

```
  for (uint256 i = 0; i < _input.length;) {
    output[i] = _input[_input.length - 1 - i];

    unchecked {
        ++i;
     }
  }
```





## Function Order Affects Gas Consumption
public/external function names and public member variable names can be optimized to save gas. See [this](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, per sorted position shifted