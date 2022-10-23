## Issues found

### Cache Array Length Outside of Loop

```
contracts/libraries/JBIpfsDecoder.sol::75 => uint8[] memory output = new uint8[](_input.length);
contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
contracts/libraries/JBIpfsDecoder.sol::77 => output[i] = _input[_input.length - 1 - i];
contracts/libraries/JBIpfsDecoder.sol::83 => bytes memory output = new bytes(_indices.length);
contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```

### Use != 0 instead of > 0 for Unsigned Integer Comparison

```
contracts/JBTiered721Delegate.sol::240 => if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);
contracts/JBTiered721DelegateStore.sol::1254 => if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
contracts/abstract/JB721Delegate.sol::116 => if (_data.tokenCount > 0) revert UNEXPECTED_TOKEN_REDEEMED();
contracts/libraries/JBIpfsDecoder.sol::57 => while (carry > 0) {
```

### Use Shift Right/Left instead of Division/Multiplication if possible

```
contracts/libraries/JBBitmap.sol::74 => return _index / 256;
contracts/libraries/JBIpfsDecoder.sol::52 => carry += uint256(digits[j]) * 256;
```