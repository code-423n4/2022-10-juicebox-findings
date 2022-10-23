# Juicebox Contest Gas Optimization Report

## Summary

The following gas optimization issues were found during the code audit:

1. Use `calldata` instead of `memory` (23 instances)
2. Cache `<array>.length` (3 instances)
3. Use `unchecked{}` to suppress overflow/underflow check (5 instances)
4. Using `bool`s for storage incurs overhead (5 instances)
5. Use `!= 0` instead of `> 0` when comparing uint (4 instances)
6. Don't initialize variables with default value (5 instances)
7. Use `++i`/`--i` instead of `i++`/`i--` (3 instances)
8. Use shift right/left instead of division/multiplication if possible (2 instances)

Total 50 instances of 8 issues.

## 1. Use `calldata` instead of `memory` (23 instances)

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for loop to copy each index of the `calldata` to the `memory` index. This overhead can be optimized by using `calldata` directly.

```solidity
contracts/JBTiered721DelegateProjectDeployer.sol::191 => function _launchProjectFor(address _owner, JBLaunchProjectData memory _launchProjectData)

contracts/JB721TieredGovernance.sol::147 => function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)

contracts/JBTiered721Delegate.sol::138 => function tokenURI(uint256 _tokenId) public view override returns (string memory) {

contracts/JBTiered721Delegate.sol::162 => function contractURI() external view override returns (string memory) {

contracts/JBTiered721Delegate.sol::264 => function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)

contracts/JBTiered721Delegate.sol::290 => function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)

contracts/JBTiered721Delegate.sol::386 => function setBaseUri(string memory _baseUri) external override onlyOwner {

contracts/JBTiered721Delegate.sol::480 => function mintFor(uint16[] memory _tierIds, address _beneficiary)

contracts/JBTiered721Delegate.sol::598 => function _didBurn(uint256[] memory _tokenIds) internal override {

contracts/JBTiered721Delegate.sol::695 => function _redemptionWeightOf(uint256[] memory _tokenIds)

contracts/JBTiered721DelegateStore.sol::279 => function tier(address _nft, uint256 _id) external view override returns (JB721Tier memory) {

contracts/JBTiered721DelegateStore.sol::467 => function flagsOf(address _nft) external view override returns (JBTiered721Flags memory) {

contracts/JBTiered721DelegateStore.sol::628 => function recordAddTiers(JB721TierParams[] memory _tiersToAdd)

contracts/JBTiered721DelegateStore.sol::1091 => function recordBurn(uint256[] memory _tokenIds) external override {

contracts/abstract/JB721Delegate.sol::311 => function _didBurn(uint256[] memory _tokenIds) internal virtual {

contracts/abstract/JB721Delegate.sol::323 => function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {

contracts/libraries/JBBitmap.sol::29 => function isTierIdRemoved(JBBitmapWord memory self, uint256 _index) internal pure returns (bool) {

contracts/libraries/JBBitmap.sol::59 => function refreshBitmapNeeded(JBBitmapWord memory self, uint256 _index)

contracts/libraries/JBIpfsDecoder.sol::22 => function decode(string memory _baseUri, bytes32 _hexString)

contracts/libraries/JBIpfsDecoder.sol::44 => function _toBase58(bytes memory _source) private pure returns (string memory) {

contracts/libraries/JBIpfsDecoder.sol::66 => function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {

contracts/libraries/JBIpfsDecoder.sol::74 => function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {

contracts/libraries/JBIpfsDecoder.sol::82 => function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```

## 2. Cache `<array>.length` (3 instances)

If `<array>.length` is used as for loop termination condition, then the `.length` method will be called in each iteration. Caching it in a local variable can save gas.

```solidity
contracts/libraries/JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {

contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {

contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```

## 3. Use `unchecked{}` to suppress overflow/underflow check (5 instances)

Starting from version 0.8.0, Solidity does overflow/underflow checks by default. It is a good feature to prevent vulnerabilities but it has a significant overhead, especially when used in for loop. When using uint256/int256, it is extremely hard to trigger overflow, so it makes sense to skip these checks. To suppress the overflow/underflow checks, use `unchecked {}`. For increment situations, just use `unchecked {}` directly; for decrement situations, add a `require()` statement before decrementing to prevent underflow.

```solidity
contracts/libraries/JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {

contracts/libraries/JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {

contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {

contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {

contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```

## 4. Using `bool`s for storage incurs overhead (5 instances)

Use `uint256(1)` and `uint256(2)` for true/false. Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

```solidity
contracts/JBTiered721Delegate.sol::543 => bool _expectMintFromExtraFunds;

contracts/JBTiered721Delegate.sol::546 => bool _dontOverspend;

contracts/JBTiered721Delegate.sol::555 => bool _dontMint;

contracts/JBTiered721Delegate.sol::616 => bool _expectMint

contracts/JBTiered721DelegateStore.sol::1015 => bool _isManualMint
```

## 5. Use `!= 0` instead of `> 0` when comparing uint (4 instances)

When dealing with unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`.

```solidity
contracts/JBTiered721Delegate.sol::240 => if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);

contracts/JBTiered721DelegateStore.sol::1254 => if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)

contracts/abstract/JB721Delegate.sol::116 => if (_data.tokenCount > 0) revert UNEXPECTED_TOKEN_REDEEMED();

contracts/libraries/JBIpfsDecoder.sol::57 => while (carry > 0) {
```

## 6. Don't initialize variables with default value (5 instances)

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```solidity
contracts/libraries/JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {

contracts/libraries/JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {

contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {

contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {

contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```

## 7. Use `++i`/`--i` instead of `i++`/`i--` (3 instances)

Using `++i`/`--i` saves 6 gas per loop.

```solidity
contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {

contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {

contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```

## 8. Use shift right/left instead of division/multiplication if possible (2 instances)

A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left. While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

```solidity
contracts/libraries/JBBitmap.sol::74 => return _index / 256;

contracts/libraries/JBIpfsDecoder.sol::52 => carry += uint256(digits[j]) * 256;
```
