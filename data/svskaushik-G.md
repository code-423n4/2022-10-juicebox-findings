## Gas Findings

### [G-01] Cache Array Length Outside of Loop
#### Impact
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack. Caching the array length in the stack saves around 3 gas per iteration.
#### Findings:
```solidity
juice-nft-rewards\JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {
juice-nft-rewards\JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
juice-nft-rewards\JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```
#### Recommendation
Store the array’s length in a variable before the for-loop.

### [G-02] No need to initialize variables with default values
#### Impact
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.
#### Findings:
```solidity
juice-nft-rewards\JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {
juice-nft-rewards\JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {
juice-nft-rewards\JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
juice-nft-rewards\JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
juice-nft-rewards\JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```
#### Recommendation
Remove explicit default initializations.

### [G-03] `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as is the case when used in `for/while` loops
#### Impact
This saves 30-40 gas [per loop](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)

#### Findings:
```solidity
juice-nft-rewards\JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {
juice-nft-rewards\JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {
juice-nft-rewards\JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
juice-nft-rewards\JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
juice-nft-rewards\JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```
#### Recommendation
Perform incremntation inside `unchecked{}`.

### [G-04] `++i` costs less gas compared to `i++` or `i += 1`
#### Impact
`++i` costs less gas compared to `i++` or `i += 1` for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.
#### Findings:
```solidity
juice-nft-rewards\JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
juice-nft-rewards\JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
juice-nft-rewards\JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
juice-nft-rewards\JBTiered721DelegateStore.sol::839 => _storedTier.initialQuantity - --_storedTier.remainingQuantity + _numberOfBurnedFromTier
juice-nft-rewards\JBTiered721DelegateStore.sol::874 => --tierBalanceOf[msg.sender][_from][_tierId];
juice-nft-rewards\JBTiered721DelegateStore.sol::991 => --_bestStoredTier.remainingQuantity +
juice-nft-rewards\JBTiered721DelegateStore.sol::1071 => --_storedTier.remainingQuantity +
```
#### Recommendation
Use `++i` instead of `i++` to increment the value of an uint variable. Same thing for `--i` and `i--`.

### [G-05] Use Shift Right/Left instead of Division/Multiplication if possible
#### Impact
A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left. While the `DIV` opcode uses 5 gas, the `SHR` opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.
#### Findings:
```solidity
juice-nft-rewards\JBBitmap.sol::74 => return _index / 256;
juice-nft-rewards\JBIpfsDecoder.sol::52 => carry += uint256(digits[j]) * 256;
```
#### Recommendation
Use SHR/SHL.
Bad
```solidity
uint256 b = a / 2
uint256 c = a / 4;
uint256 d = a * 8;
```
Good
```solidity
uint256 b = a >> 1;
uint256 c = a >> 2;
uint256 d = a << 3;
```

### [G-06] Contracts using unlocked pragma.
#### Impact
Contracts in scope use `pragma solidity ^0.X.Y` or `pragma solidity >0.X.Y`, allowing wide range of versions.
#### Findings:
```solidity
juice-nft-rewards\JB721Delegate.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JB721GlobalGovernance.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JB721TieredGovernance.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBBitmap.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBIpfsDecoder.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721Delegate.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721DelegateDeployer.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721DelegateProjectDeployer.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721DelegateStore.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721FundingCycleMetadataResolver.sol::2 => pragma solidity ^0.8.16;
```
#### Recommendation
Consider locking compiler version, for example `pragma solidity 0.8.16`. 

### [G-07] Use `calldata` instead of `memory` for read-only arguments in `external` functions.
#### Impact
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution.
#### Findings:
```solidity
juice-nft-rewards\JBTiered721Delegate.sol::386 => function setBaseUri(string memory _baseUri) external override onlyOwner {
juice-nft-rewards\JBTiered721DelegateStore.sol::1091 => function recordBurn(uint256[] memory _tokenIds) external override {
```
#### Recommendation
Use `calldata` instead of `memory`.

### [G-08] Use `storage` instead of `memory` for structs/arrays.
#### Impact
When fetching data from a `storage` location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from `storage`, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another `memory` array/struct.
#### Findings:
```solidity
juice-nft-rewards\JBTiered721DelegateStore.sol::234 => JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_currentSortIndex);
juice-nft-rewards\JBTiered721DelegateStore.sol::281 => JBStored721Tier memory _storedTier = _storedTierOf[_nft][_id];
juice-nft-rewards\JBTiered721DelegateStore.sol::317 => JBStored721Tier memory _storedTier = _storedTierOf[_nft][_tierId];
juice-nft-rewards\JBTiered721DelegateStore.sol::481 => JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_tierId);
juice-nft-rewards\JBTiered721DelegateStore.sol::657 => JBTiered721Flags memory _flags = _flagsOf[msg.sender];
juice-nft-rewards\JBTiered721DelegateStore.sol::717 => JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_currentSortIndex);
juice-nft-rewards\JBTiered721DelegateStore.sol::948 => JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_currentSortIndex);
juice-nft-rewards\JBTiered721DelegateStore.sol::1033 => JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_tierIds[0]);
juice-nft-rewards\JBTiered721DelegateStore.sol::1184 => JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_currentSortIndex);
```
#### Recommendation
Use `storage` instead of `memory` for findings above