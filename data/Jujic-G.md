Gas report ( 7 findings with  20 instances )
## 1. > 0 can be replaced with != 0 for gas optimisation
*There are 2 instances of this issue:* 
```
if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L240
```
if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1254

## 2. Add unchecked {} for subtractions where the operands cannot underflow because of a previous require() or if-statement
*There are 4 instances of this issue:* 
```
projectId = controller.projects().count() + 1;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L76
```
return _index + 1;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1303


```
for (uint256 i = 0; i < _input.length; i++)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76
```
for (uint256 i = 0; i < _indices.length; i++) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84



## 3. Prefix increments are cheaper than postfix increments

*There are 2 instances of this issue:* 
```
numberOfBurnedFor[msg.sender][_tierId]++;

_storedTierOf[msg.sender][_tierId].remainingQuantity++;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1106
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1108

## 4. x += y consumes more gas than x = x + y for state variables
```
numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L827

## 5. A division by 256 can be calculated by shifting
*There are 2 instances of this issue:* 
```
function _retrieveDepth(uint256 _index) internal pure returns (uint256) {
    return _index / 256;
  }
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L74
```
carry += uint256(digits[j]) * 256;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L52


## 6. Cashe array length in loops
*There are 4 instances of this issue:* 
```
for (uint256 i = 0; i < _source.length; ++i)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49

```
for (uint256 i = 0; i < _length; i++) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68
```
for (uint256 i = 0; i < _input.length; i++)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76
```
for (uint256 i = 0; i < _indices.length; i++) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84

## 7. A variable is being assigned its default value which is unnecessary.
*There are 5 instances of this issue:* 
```
for (uint256 i = 0; i < _source.length; ++i)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49

```
for (uint256 i = 0; i < _length; i++) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68
```
for (uint256 i = 0; i < _input.length; i++)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76
```
for (uint256 i = 0; i < _indices.length; i++) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84
```
for (uint256 j = 0; j < digitlength; ++j) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L51

