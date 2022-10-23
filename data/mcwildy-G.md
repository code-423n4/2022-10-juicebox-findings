# GAS OPTIMIZATIONS REPORT
## 2022-10-JUICEBOX

### Do not write default values to variables
If you declare a variables of type `uint256` it will automatically get assigned to its default value of 0. It is a gas wastage to assign it yourself.

[JBIpfsDecoder.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol):
```solidity
line#L49:  for (uint256 i = 0; i < _source.length; ++i) {

line#L51:  for (uint256 j = 0; j < digitlength; ++j) {

line#L76:  for (uint256 i = 0; i < _input.length; i++) {

line#L84:  for (uint256 i = 0; i < _indices.length; i++) {
```

### Use `constant` and `immutable` keywords for storage varibles if they doesn't change through contract's lifecycle
That way the variable will go to the contract's bytecode and will not allocate a storage slot

[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol):
```solidity
line#L48:  address public override codeOrigin;
```

### `++i` costs less gas than `i++` (same for `--i`/`i--`)
Prefix increments are cheaper than postfix increments

[JBTiered721DelegateStore.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol):
```solidity
line#L1108:_storedTierOf[msg.sender][_tierId].remainingQuantity++;
```

[JBIpfsDecoder.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol):
```solidity
line#L59:  digitlength++;

line#L68:  for (uint256 i = 0; i < _length; i++) {

line#L76:  for (uint256 i = 0; i < _input.length; i++) {

line#L84:  for (uint256 i = 0; i < _indices.length; i++) {
```

### Mark functions as `payable` to avoid the low-level evm `is-a-payment-provided` check
The EVM checks whether a payment is provided to the function and this check costs additional gas. If the function is marked as `payable` there is no such check

[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol):
```solidity
line#L370: function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {

line#L386: function setBaseUri(string memory _baseUri) external override onlyOwner {

line#L402: function setContractUri(string calldata _contractUri) external override onlyOwner {

line#L418: function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {
```

### Function inlining
If a function is called only once it can be inlined in order to save 20 gas

[JBIpfsDecoder.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol):
```solidity
line#L44:  function _toBase58(bytes memory _source) private pure returns (string memory) {

line#L66:  function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {

line#L74:  function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {

line#L82:  function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```

### Use x = x + y instead of x += y for storage variables


[JBTiered721DelegateStore.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol):
```solidity
line#L827: numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

### `x < y + 1` is cheaper than `x <= y`
 

[JBTiered721DelegateStore.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol):
```solidity
line#L903: if (_storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();
```

[JB721TieredGovernance.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol):
```solidity
line#L133: if (_blockNumber >= block.number) revert BLOCK_NOT_YET_MINED();
```

### Use `unchecked` for the counter in `for` loops
Since it is compared to a target value (usually the length of an array) there is no way the counter overflows

[JBIpfsDecoder.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol):
```solidity
line#L49:  for (uint256 i = 0; i < _source.length; ++i) {

line#L51:  for (uint256 j = 0; j < digitlength; ++j) {

line#L76:  for (uint256 i = 0; i < _input.length; i++) {

line#L84:  for (uint256 i = 0; i < _indices.length; i++) {
```

[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol):
```solidity
line#L341: ++_i;

line#L355: ++_i;
```

### Use `calldata` where possible instead of `memory` to save gas


[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721Delegate.sol):
```solidity
line#L205: string memory _name,

line#L206: string memory _symbol,

line#L210: string memory _contractUri,

line#L211: JB721PricingParams memory _pricing,

line#L264: function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)

line#L290: function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)

line#L480: function mintFor(uint16[] memory _tierIds, address _beneficiary)

line#L598: function _didBurn(uint256[] memory _tokenIds) internal override {

line#L695: function _redemptionWeightOf(uint256[] memory _tokenIds)

line#L789: JB721Tier memory _tier
```

[JBIpfsDecoder.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol):
```solidity
line#L22:  function decode(string memory _baseUri, bytes32 _hexString)

           /// @audit Store `_source` in calldata.
line#L44:  function _toBase58(bytes memory _source) private pure returns (string memory) {

           /// @audit Store `_input` in calldata.
line#L74:  function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {

           /// @audit Store `_indices` in calldata.
line#L82:  function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```

[JB721GlobalGovernance.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721GlobalGovernance.sol):
```solidity
line#L55:  JB721Tier memory _tier
```

[JBTiered721DelegateStore.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol):
```solidity
line#L628: function recordAddTiers(JB721TierParams[] memory _tiersToAdd)

line#L1091:function recordBurn(uint256[] memory _tokenIds) external override {

line#L1227:JBStored721Tier memory _storedTier
```

[JBTiered721DelegateProjectDeployer.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateProjectDeployer.sol):
```solidity
line#L72:  JBDeployTiered721DelegateData memory _deployTiered721DelegateData,

line#L73:  JBLaunchProjectData memory _launchProjectData

line#L110: JBLaunchFundingCyclesData memory _launchFundingCyclesData

line#L152: JBDeployTiered721DelegateData memory _deployTiered721DelegateData,

line#L191: function _launchProjectFor(address _owner, JBLaunchProjectData memory _launchProjectData)

line#L218: JBLaunchFundingCyclesData memory _launchFundingCyclesData
```

[JBBitmap.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBBitmap.sol):
```solidity
line#L29:  function isTierIdRemoved(JBBitmapWord memory self, uint256 _index) internal pure returns (bool) {

line#L59:  function refreshBitmapNeeded(JBBitmapWord memory self, uint256 _index)
```

[JB721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/abstract/JB721Delegate.sol):
```solidity
line#L206: string memory _name,

line#L207: string memory _symbol

line#L311: function _didBurn(uint256[] memory _tokenIds) internal virtual {

line#L323: function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {
```

[JBTiered721DelegateDeployer.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateDeployer.sol):
```solidity
line#L71:  JBDeployTiered721DelegateData memory _deployTiered721DelegateData
```

[JB721TieredGovernance.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JB721TieredGovernance.sol):
```solidity
line#L147: function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)

line#L313: JB721Tier memory _tier
```

### Use `uint256` instead of the smaller uints where possible
The word-size in ethereum is 32 bytes which means that every variables which is sized with less bytes will cost more to operate with

[JBIpfsDecoder.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol):
```solidity
line#L48:  uint8 digitlength = 1;

line#L66:  function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
```

### Use `if (x != 0)` instead of `if (x > 0)` for uints comparison
Saves 3 gas per `if`-check

[JBIpfsDecoder.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/libraries/JBIpfsDecoder.sol):
```solidity
line#L57:  while (carry > 0) {
```

[JBTiered721DelegateStore.sol](https://github.com/jbx-protocol/juice-nft-rewards/tree/main/contracts/JBTiered721DelegateStore.sol):
```solidity
line#L1254:if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
```

