## [G001] >= costs less gas than >

he compiler uses opcodes `GT` and `ISZERO` for solidity code that uses `>`, but only requires `LT` for `>=`, which saves 3 gas

#### Instances:
```
contracts/JB721TieredGovernance.sol::133 => if (_blockNumber >= block.number) revert BLOCK_NOT_YET_MINED();
contracts/JBTiered721Delegate.sol::240 => if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);
contracts/JBTiered721DelegateStore.sol::22 => using JBBitmap for mapping(uint256=>uint256);
contracts/JBTiered721DelegateStore.sol::760 => else if (_next == 0 || _next > _currentMaxTierIdOf) {
contracts/JBTiered721DelegateStore.sol::824 => if (_count > _numberOfReservedTokensOutstanding) revert INSUFFICIENT_RESERVES();
contracts/JBTiered721DelegateStore.sol::903 => if (_storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();
contracts/JBTiered721DelegateStore.sol::959 => if (_storedTier.contributionFloor > _amount) _currentSortIndex = 0;
contracts/JBTiered721DelegateStore.sol::1056 => if (_storedTier.contributionFloor > leftoverAmount) revert INSUFFICIENT_AMOUNT();
contracts/JBTiered721DelegateStore.sol::1254 => if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
contracts/abstract/JB721Delegate.sol::116 => if (_data.tokenCount > 0) revert UNEXPECTED_TOKEN_REDEEMED();
```

## [G002] Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution. Structs have the same overhead as an array of length one

#### Instances:
```
contracts/JBTiered721DelegateStore.sol::1091 => function recordBurn(uint256[] memory _tokenIds) external override {
```
## [G003] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

#### Instances:
```
contracts/JBTiered721DelegateStore.sol::354 => supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;
contracts/JBTiered721DelegateStore.sol::409 => units += _balance * _storedTierOf[_nft][_i].votingUnits;
contracts/JBTiered721DelegateStore.sol::506 => balance += tierBalanceOf[_nft][_owner][_i];
contracts/JBTiered721DelegateStore.sol::534 => weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
contracts/JBTiered721DelegateStore.sol::563 => weight +=
contracts/JBTiered721DelegateStore.sol::827 => numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

## [G004] `++i`/`i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as is the case when used in `for`- and `while`-loops

The `unchecked` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

#### Instances:
```
contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```
## [G005] Using `bools` for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.Refer https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27 . Use uint256(1) and uint256(2) for true/false

#### Instances:
```
contracts/JBTiered721Delegate.sol::543 => bool _expectMintFromExtraFunds;
contracts/JBTiered721Delegate.sol::546 => bool _dontOverspend;
```

## [G006] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.\[layout_in_storage.html]{https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html}\Use a larger size then downcast where needed

#### Instances:
```
contracts/libraries/JBIpfsDecoder.sol::46 => uint8[] memory digits = new uint8[](46); // hash size with the prefix
contracts/libraries/JBIpfsDecoder.sol::48 => uint8 digitlength = 1;
contracts/libraries/JBIpfsDecoder.sol::67 => uint8[] memory output = new uint8[](_length);
contracts/libraries/JBIpfsDecoder.sol::75 => uint8[] memory output = new uint8[](_input.length);
```

## [G007] Functions with access control cheaper if payable

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are `CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

#### Instances:
```
contracts/JBTiered721Delegate.sol::370 => function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {
contracts/JBTiered721Delegate.sol::386 => function setBaseUri(string memory _baseUri) external override onlyOwner {
contracts/JBTiered721Delegate.sol::402 => function setContractUri(string calldata _contractUri) external override onlyOwner {
contracts/JBTiered721Delegate.sol::418 => function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {
```
