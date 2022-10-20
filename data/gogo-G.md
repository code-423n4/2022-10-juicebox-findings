# JUICE-NFT-REWARDS
## Gas Optimizations Report

### The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.
This change would save up to 6 gas per instance/loop.

_There are **5** instances of this issue:_

```solidity
File: contracts/JBTiered721DelegateStore.sol

1108: _storedTierOf[msg.sender][_tierId].remainingQuantity++;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol

```solidity
File: contracts/libraries/JBIpfsDecoder.sol

59:    digitlength++;

68:   for (uint256 i = 0; i < _length; i++) {

76:   for (uint256 i = 0; i < _input.length; i++) {

84:   for (uint256 i = 0; i < _indices.length; i++) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol

### State variables should be cached in stack variables rather than re-reading them.
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There are **1** instances of this issue:_

```solidity
File: contracts/JBTiered721Delegate.sol

      /// @audit Cache `store`. Used 3 times in `tokenURI`
143:  IJBTokenUriResolver _resolver = store.tokenUriResolverOf(address(this));
151:  store.baseUriOf(address(this)),
152:  store.encodedTierIPFSUriOf(address(this), _tokenId)
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

### `internal` and `private` functions that are called only once should be inlined.
The execution of a non-inlined function would cost up to 40 more gas because of two extra `jump`s as well as some other instructions.

_There are **4** instances of this issue:_

```solidity
File: contracts/libraries/JBIpfsDecoder.sol

44:   function _toBase58(bytes memory _source) private pure returns (string memory) {

66:   function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {

74:   function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {

82:   function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol

### Using `!= 0` on `uints` costs less gas than `> 0`.
This change saves 3 gas per instance/loop

_There are **2** instances of this issue:_

```solidity
File: contracts/JBTiered721DelegateStore.sol

1254: if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol

```solidity
File: contracts/libraries/JBIpfsDecoder.sol

57:   while (carry > 0) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol

### It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied
Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **5** instances of this issue:_

```solidity
File: contracts/libraries/JBIpfsDecoder.sol

49:   for (uint256 i = 0; i < _source.length; ++i) {

51:   for (uint256 j = 0; j < digitlength; ++j) {

68:   for (uint256 i = 0; i < _length; i++) {

76:   for (uint256 i = 0; i < _input.length; i++) {

84:   for (uint256 i = 0; i < _indices.length; i++) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol

### Functions that are access-restricted from most users may be marked as `payable`
Marking a function as `payable` reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **4** instances of this issue:_

```solidity
File: contracts/JBTiered721Delegate.sol

370:  function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {

386:  function setBaseUri(string memory _baseUri) external override onlyOwner {

402:  function setContractUri(string calldata _contractUri) external override onlyOwner {

418:  function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

### `++i`/`i++` should be `unchecked{++I}`/`unchecked{I++}` in `for`-loops
When an increment or any arithmetic operation is not possible to overflow it should be placed in `unchecked{}` block. \This is because of the default compiler overflow and underflow safety checks since Solidity version 0.8.0. \In for-loops it saves around 30-40 gas **per loop**

_There are **7** instances of this issue:_

```solidity
File: contracts/JBTiered721Delegate.sol

341:    ++_i;

355:    ++_i;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

```solidity
File: contracts/libraries/JBIpfsDecoder.sol

49:   for (uint256 i = 0; i < _source.length; ++i) {

51:   for (uint256 j = 0; j < digitlength; ++j) {

68:   for (uint256 i = 0; i < _length; i++) {

76:   for (uint256 i = 0; i < _input.length; i++) {

84:   for (uint256 i = 0; i < _indices.length; i++) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol

### `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables


_There are **1** instances of this issue:_

```solidity
File: contracts/JBTiered721DelegateStore.sol

827:  numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol

### Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead
'When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.' \ https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html \ Use a larger size then downcast where needed

_There are **2** instances of this issue:_

```solidity
File: contracts/libraries/JBIpfsDecoder.sol

48:   uint8 digitlength = 1;

66:   function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol

### Use `calldata` instead of `memory` for function parameters
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **41** instances of this issue:_

```solidity
File: contracts/JB721GlobalGovernance.sol

55:   JB721Tier memory _tier
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721GlobalGovernance.sol

```solidity
File: contracts/JB721TieredGovernance.sol

147:  function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)

313:  JB721Tier memory _tier
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol

```solidity
File: contracts/JBTiered721Delegate.sol

205:  string memory _name,

206:  string memory _symbol,

208:  string memory _baseUri,

210:  string memory _contractUri,

211:  JB721PricingParams memory _pricing,

213:  JBTiered721Flags memory _flags

264:  function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)

290:  function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)

386:  function setBaseUri(string memory _baseUri) external override onlyOwner {

480:  function mintFor(uint16[] memory _tierIds, address _beneficiary)

598:  function _didBurn(uint256[] memory _tokenIds) internal override {

652:  uint16[] memory _mintTierIds,

695:  function _redemptionWeightOf(uint256[] memory _tokenIds)

789:  JB721Tier memory _tier
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

```solidity
File: contracts/JBTiered721DelegateDeployer.sol

71:   JBDeployTiered721DelegateData memory _deployTiered721DelegateData
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateDeployer.sol

```solidity
File: contracts/JBTiered721DelegateProjectDeployer.sol

72:   JBDeployTiered721DelegateData memory _deployTiered721DelegateData,

73:   JBLaunchProjectData memory _launchProjectData

109:  JBDeployTiered721DelegateData memory _deployTiered721DelegateData,

110:  JBLaunchFundingCyclesData memory _launchFundingCyclesData

152:  JBDeployTiered721DelegateData memory _deployTiered721DelegateData,

153:  JBReconfigureFundingCyclesData memory _reconfigureFundingCyclesData

191:  function _launchProjectFor(address _owner, JBLaunchProjectData memory _launchProjectData)

218:  JBLaunchFundingCyclesData memory _launchFundingCyclesData

244:  JBReconfigureFundingCyclesData memory _reconfigureFundingCyclesData
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol

```solidity
File: contracts/JBTiered721DelegateStore.sol

628:  function recordAddTiers(JB721TierParams[] memory _tiersToAdd)

1091: function recordBurn(uint256[] memory _tokenIds) external override {

1227: JBStored721Tier memory _storedTier
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol

```solidity
File: contracts/abstract/JB721Delegate.sol

206:  string memory _name,

207:  string memory _symbol

311:  function _didBurn(uint256[] memory _tokenIds) internal virtual {

323:  function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol

```solidity
File: contracts/libraries/JBBitmap.sol

29:   function isTierIdRemoved(JBBitmapWord memory self, uint256 _index) internal pure returns (bool) {

59:   function refreshBitmapNeeded(JBBitmapWord memory self, uint256 _index)
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBBitmap.sol

```solidity
File: contracts/libraries/JBIpfsDecoder.sol

22:   function decode(string memory _baseUri, bytes32 _hexString)

      /// @audit Store `_source` in calldata.
44:   function _toBase58(bytes memory _source) private pure returns (string memory) {

      /// @audit Store `_array` in calldata.
66:   function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {

      /// @audit Store `_input` in calldata.
74:   function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {

      /// @audit Store `_indices` in calldata.
82:   function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`
In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **2** instances of this issue:_

```solidity
File: contracts/JB721TieredGovernance.sol

133:  if (_blockNumber >= block.number) revert BLOCK_NOT_YET_MINED();
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol

```solidity
File: contracts/JBTiered721DelegateStore.sol

903:  if (_storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol

### Use `immutable` & `constant` for state variables that do not change their value


_There are **1** instances of this issue:_

```solidity
File: contracts/JBTiered721Delegate.sol

48:   address public override codeOrigin;
```
https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol

