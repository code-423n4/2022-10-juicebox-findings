## Gas Optimizations
 *** 


### G-01: pre-increment `++i/--i` costs less gas than post-increment `i++/i--`
Saves 6 gas per loop in a for loop

Total instances of this issue: 3

```solidity
contracts/libraries/JBIpfsDecoder.sol

68:    for (uint256 i = 0; i < _length; i++) {

76:    for (uint256 i = 0; i < _input.length; i++) {

84:    for (uint256 i = 0; i < _indices.length; i++) {


```
 *** 


### G-02: `++i/i++` should be placed in unchecked blocks to save gas as it is impossible for them to overflow in for and while loops
Unchecked keyword is available in solidity version `0.8.0` or higher and can be applied to iterator variables to save gas. 
Saves more than `30 gas` per loop.

Total instances of this issue: 4

```solidity
contracts/libraries/JBIpfsDecoder.sol

49:    for (uint256 i = 0; i < _source.length; ++i) {

68:    for (uint256 i = 0; i < _length; i++) {

76:    for (uint256 i = 0; i < _input.length; i++) {

84:    for (uint256 i = 0; i < _indices.length; i++) {


```
 *** 


### G-03: Length of the array (`<array>.length`) need not be looked up in every iteration of a for-loop
Reading array length at each iteration of the loop takes total 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the `array.length` saves around `3 gas` per iteration.

Total instances of this issue: 3

```solidity
contracts/libraries/JBIpfsDecoder.sol

49:    for (uint256 i = 0; i < _source.length; ++i) {

76:    for (uint256 i = 0; i < _input.length; i++) {

84:    for (uint256 i = 0; i < _indices.length; i++) {


```
 *** 


### G-04: `x += y` costs more gas than `x = x + y` for state variables

Total instances of this issue: 7

```solidity
contracts/JBTiered721DelegateStore.sol

354:      supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;

409:        units += _balance * _storedTierOf[_nft][_i].votingUnits;

506:      balance += tierBalanceOf[_nft][_owner][_i];

534:      weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;

563:      weight +=
564:        (_storedTier.contributionFloor *
565:          (_storedTier.initialQuantity - _storedTier.remainingQuantity)) +
566:        _numberOfReservedTokensOutstandingFor(_nft, _i, _storedTier);

827:    numberOfReservesMintedFor[msg.sender][_tierId] += _count;


```
```solidity
contracts/libraries/JBIpfsDecoder.sol

52:        carry += uint256(digits[j]) * 256;


```
 *** 


### G-05: Adding `payable` to functions which are only meant to be called by specific actors like `onlyOwner` will save gas cost
Marking functions payable removes additional checks for whether a payment was provided, hence reducing gas cost

Total instances of this issue: 7

```solidity
contracts/JBTiered721Delegate.sol

290:  function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)
291:    external
292:    override
293:    onlyOwner
294:  {

321:  function adjustTiers(JB721TierParams[] calldata _tiersToAdd, uint256[] calldata _tierIdsToRemove)
322:    external
323:    override
324:    onlyOwner
325:  {

370:  function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {

386:  function setBaseUri(string memory _baseUri) external override onlyOwner {

402:  function setContractUri(string calldata _contractUri) external override onlyOwner {

418:  function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {

480:  function mintFor(uint16[] memory _tierIds, address _beneficiary)
481:    public
482:    override
483:    onlyOwner
484:    returns (uint256[] memory tokenIds)
485:  {


```
 *** 


### G-06: No need to initialize non-constant/non-immutable variables to zero
Since the default value is already zero, overwriting is not required.
Saves `8 gas` per instance.

Total instances of this issue: 5

```solidity
contracts/libraries/JBIpfsDecoder.sol

49:    for (uint256 i = 0; i < _source.length; ++i) {

51:      for (uint256 j = 0; j < digitlength; ++j) {

68:    for (uint256 i = 0; i < _length; i++) {

76:    for (uint256 i = 0; i < _input.length; i++) {

84:    for (uint256 i = 0; i < _indices.length; i++) {


```
 *** 


### G-07: Using uints/ints smaller than 256 bits increases overhead
Gas usage becomes higher with uint/int smaller than 256 bits because EVM operates on 32 bytes and uses additional operations to reduce the size from 32 bytes to the target size.

Total instances of this issue: 2

```solidity
contracts/libraries/JBIpfsDecoder.sol

48:    uint8 digitlength = 1;

66:  function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {


```
 *** 


### G-08: Using custom errors rather than revert()/require() strings will save deployment gas
Custom errors are available from solidity version 0.8.4.

Total instances of this issue: 2

```solidity
contracts/JBTiered721Delegate.sol

216:    require(address(this) != codeOrigin);

218:    require(address(store) == address(0));


```
