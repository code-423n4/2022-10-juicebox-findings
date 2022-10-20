# GAS ISSUES FOR JUICE-NFT-REWARDS

##  [G-01]  Use `++i` instead of `i++`


./contracts/JBTiered721DelegateStore.sol
```
L1108: _storedTierOf[msg.sender][_tierId].remainingQuantity++;
```

./contracts/libraries/JBIpfsDecoder.sol
```
L68:   for (uint256 i = 0; i < _length; i++) {

L76:   for (uint256 i = 0; i < _input.length; i++) {

L84:   for (uint256 i = 0; i < _indices.length; i++) {
```

##  [G-02]  `uncheck` the `i++`/`i--` in for loops since there's no way to overflow/underflow


./contracts/JBTiered721Delegate.sol
```
L341:  ++_i;

L355:  ++_i;
```

./contracts/libraries/JBIpfsDecoder.sol
```
L51:   for (uint256 j = 0; j < digitlength; ++j) {

L68:   for (uint256 i = 0; i < _length; i++) {

L76:   for (uint256 i = 0; i < _input.length; i++) {

L84:   for (uint256 i = 0; i < _indices.length; i++) {
```

##  [G-03]  `!= 0` comparison is cheaper than `> 0`


./contracts/libraries/JBIpfsDecoder.sol
```
L57:   while (carry > 0) {
```

./contracts/JBTiered721DelegateStore.sol
```
L1254: if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
```

##  [G-04]  A=A+B costs less gas than A+=B if A and B are located in storage


./contracts/JBTiered721DelegateStore.sol
```
L827:  numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

##  [G-05]  Use default values of variables types
Description: uint256 - 0;, string - "";, address - address(0);, etc.

./contracts/libraries/JBIpfsDecoder.sol
```
L49:   for (uint256 i = 0; i < _source.length; ++i) {

L68:   for (uint256 i = 0; i < _length; i++) {

L76:   for (uint256 i = 0; i < _input.length; i++) {

L84:   for (uint256 i = 0; i < _indices.length; i++) {
```

##  [G-06]  Inline internal functions to save gas


./contracts/libraries/JBIpfsDecoder.sol
```
L66:   function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {

L74:   function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {

L82:   function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```

##  [G-07]  If a function is not open to any user, it can be marked as `payable` to save gas
Description: Under the hood the compiler checks if a function is payable. The check is skipped if it is marked as such by the developer

./contracts/JBTiered721Delegate.sol
```
L370:  function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {

L386:  function setBaseUri(string memory _baseUri) external override onlyOwner {

L418:  function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {
```

##  [G-08]  A < B + 1 is cheaper than A <= B
 

./contracts/JB721TieredGovernance.sol
```
L133:  if (_blockNumber >= block.number) revert BLOCK_NOT_YET_MINED();
```

./contracts/JBTiered721DelegateStore.sol
```
L903:  if (_storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();
```

##  [G-09]  Skip copying data from calldata to memory for function parameters
Description: Use `calldata` location for reference-type input args

./contracts/JBTiered721DelegateProjectDeployer.sol
```
L72:   JBDeployTiered721DelegateData memory _deployTiered721DelegateData,

L109:  JBDeployTiered721DelegateData memory _deployTiered721DelegateData,

L152:  JBDeployTiered721DelegateData memory _deployTiered721DelegateData,

L153:  JBReconfigureFundingCyclesData memory _reconfigureFundingCyclesData

L191:  function _launchProjectFor(address _owner, JBLaunchProjectData memory _launchProjectData)

L218:  JBLaunchFundingCyclesData memory _launchFundingCyclesData

L244:  JBReconfigureFundingCyclesData memory _reconfigureFundingCyclesData
```

./contracts/abstract/JB721Delegate.sol
```
L207:  string memory _symbol

L311:  function _didBurn(uint256[] memory _tokenIds) internal virtual {

L323:  function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {
```

./contracts/JBTiered721Delegate.sol
```
L205:  string memory _name,

L208:  string memory _baseUri,

L210:  string memory _contractUri,

L211:  JB721PricingParams memory _pricing,

L213:  JBTiered721Flags memory _flags

L290:  function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)

L598:  function _didBurn(uint256[] memory _tokenIds) internal override {

L652:  uint16[] memory _mintTierIds,

L695:  function _redemptionWeightOf(uint256[] memory _tokenIds)

L789:  JB721Tier memory _tier
```

./contracts/JBTiered721DelegateStore.sol
```
L628:  function recordAddTiers(JB721TierParams[] memory _tiersToAdd)

L1091: function recordBurn(uint256[] memory _tokenIds) external override {

L1227: JBStored721Tier memory _storedTier
```

./contracts/JB721TieredGovernance.sol
```
L147:  function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)

L313:  JB721Tier memory _tier
```

./contracts/JB721GlobalGovernance.sol
```
L55:   JB721Tier memory _tier
```

./contracts/JBTiered721DelegateDeployer.sol
```
L71:   JBDeployTiered721DelegateData memory _deployTiered721DelegateData
```

./contracts/libraries/JBBitmap.sol
```
L29:   function isTierIdRemoved(JBBitmapWord memory self, uint256 _index) internal pure returns (bool) {

L59:   function refreshBitmapNeeded(JBBitmapWord memory self, uint256 _index)
```

./contracts/libraries/JBIpfsDecoder.sol
```
L22:   function decode(string memory _baseUri, bytes32 _hexString)

       /// @audit Store `_source` in calldata.
L44:   function _toBase58(bytes memory _source) private pure returns (string memory) {

       /// @audit Store `_array` in calldata.
L66:   function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {

       /// @audit Store `_input` in calldata.
L74:   function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {
```

