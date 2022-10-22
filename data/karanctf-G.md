## [G-1] Use calldata instead of memory
```solidity
JB721Delegate.sol:84:      string memory memo,
JB721Delegate.sol:85:      JBPayDelegateAllocation[] memory delegateAllocations
JB721Delegate.sol:111:      string memory memo,
JB721Delegate.sol:112:      JBRedemptionDelegateAllocation[] memory delegateAllocations
JB721Delegate.sol:133:    (, uint256[] memory _decodedTokenIds) = abi.decode(_data.metadata, (bytes4, uint256[]));
JB721Delegate.sol:206:    string memory _name,
JB721Delegate.sol:207:    string memory _symbol
JB721Delegate.sol:263:    (, uint256[] memory _decodedTokenIds) = abi.decode(_data.metadata, (bytes4, uint256[]));
JB721Delegate.sol:311:  function _didBurn(uint256[] memory _tokenIds) internal virtual {
JB721Delegate.sol:323:  function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {
JB721GlobalGovernance.sol:55:    JB721Tier memory _tier
JBTiered721DelegateDeployer.sol:71:    JBDeployTiered721DelegateData memory _deployTiered721DelegateData
JBBitmap.sol:29:  function isTierIdRemoved(JBBitmapWord memory self, uint256 _index) internal pure returns (bool) {
JBBitmap.sol:59:  function refreshBitmapNeeded(JBBitmapWord memory self, uint256 _index)
JBTiered721DelegateProjectDeployer.sol:72:    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,
JBTiered721DelegateProjectDeployer.sol:73:    JBLaunchProjectData memory _launchProjectData
JBTiered721DelegateProjectDeployer.sol:109:    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,
JBTiered721DelegateProjectDeployer.sol:110:    JBLaunchFundingCyclesData memory _launchFundingCyclesData
JBTiered721DelegateProjectDeployer.sol:152:    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,
JBTiered721DelegateProjectDeployer.sol:153:    JBReconfigureFundingCyclesData memory _reconfigureFundingCyclesData
JBTiered721DelegateProjectDeployer.sol:191:  function _launchProjectFor(address _owner, JBLaunchProjectData memory _launchProjectData)
JBTiered721DelegateProjectDeployer.sol:218:    JBLaunchFundingCyclesData memory _launchFundingCyclesData
JBTiered721DelegateProjectDeployer.sol:244:    JBReconfigureFundingCyclesData memory _reconfigureFundingCyclesData
JB721TieredGovernance.sol:147:  function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)
JB721TieredGovernance.sol:156:    JBTiered721SetTierDelegatesData memory _data;
JB721TieredGovernance.sol:313:    JB721Tier memory _tier
JBTiered721Delegate.sol:205:    string memory _name,
JBTiered721Delegate.sol:206:    string memory _symbol,
JBTiered721Delegate.sol:208:    string memory _baseUri,
JBTiered721Delegate.sol:210:    string memory _contractUri,
JBTiered721Delegate.sol:211:    JB721PricingParams memory _pricing,
JBTiered721Delegate.sol:213:    JBTiered721Flags memory _flags
JBTiered721Delegate.sol:264:  function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)
JBTiered721Delegate.sol:273:      JBTiered721MintReservesForTiersData memory _data = _mintReservesForTiersData[_i];
JBTiered721Delegate.sol:290:  function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)
JBTiered721Delegate.sol:300:      JBTiered721MintForTiersData memory _data = _mintForTiersData[_i];
JBTiered721Delegate.sol:349:      uint256[] memory _tierIdsAdded = store.recordAddTiers(_tiersToAdd);
JBTiered721Delegate.sol:386:  function setBaseUri(string memory _baseUri) external override onlyOwner {
JBTiered721Delegate.sol:438:    JBFundingCycle memory _fundingCycle = fundingCycleStore.currentOf(projectId);
JBTiered721Delegate.sol:448:    uint256[] memory _tokenIds = store.recordMintReservesFor(_tierId, _count);
JBTiered721Delegate.sol:480:  function mintFor(uint16[] memory _tierIds, address _beneficiary)
JBTiered721Delegate.sol:484:    returns (uint256[] memory tokenIds)
JBTiered721Delegate.sol:558:      uint16[] memory _tierIdsToMint;
JBTiered721Delegate.sol:598:  function _didBurn(uint256[] memory _tokenIds) internal override {
JBTiered721Delegate.sol:652:    uint16[] memory _mintTierIds,
JBTiered721Delegate.sol:656:    uint256[] memory _tokenIds;
JBTiered721Delegate.sol:695:  function _redemptionWeightOf(uint256[] memory _tokenIds)
JBTiered721Delegate.sol:733:        JBFundingCycle memory _fundingCycle = fundingCycleStore.currentOf(projectId);
JBTiered721Delegate.sol:765:    JB721Tier memory _tier = store.tierOfTokenId(address(this), _tokenId);
JBTiered721Delegate.sol:789:    JB721Tier memory _tier
JBIpfsDecoder.sol:22:  function decode(string memory _baseUri, bytes32 _hexString)
JBIpfsDecoder.sol:28:    bytes memory completeHexString = abi.encodePacked(bytes2(0x1220), _hexString);
JBIpfsDecoder.sol:31:    string memory ipfsHash = _toBase58(completeHexString);
JBIpfsDecoder.sol:44:  function _toBase58(bytes memory _source) private pure returns (string memory) {
JBIpfsDecoder.sol:46:    uint8[] memory digits = new uint8[](46); // hash size with the prefix
JBIpfsDecoder.sol:66:  function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
JBIpfsDecoder.sol:67:    uint8[] memory output = new uint8[](_length);
JBIpfsDecoder.sol:74:  function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {
JBIpfsDecoder.sol:75:    uint8[] memory output = new uint8[](_input.length);
JBIpfsDecoder.sol:82:  function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
JBIpfsDecoder.sol:83:    bytes memory output = new bytes(_indices.length);
JBTiered721DelegateStore.sol:217:  ) external view override returns (JB721Tier[] memory _tiers) {
JBTiered721DelegateStore.sol:231:    JBStored721Tier memory _storedTier;
JBTiered721DelegateStore.sol:234:    JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_currentSortIndex);
JBTiered721DelegateStore.sol:281:    JBStored721Tier memory _storedTier = _storedTierOf[_nft][_id];
JBTiered721DelegateStore.sol:317:    JBStored721Tier memory _storedTier = _storedTierOf[_nft][_tierId];
JBTiered721DelegateStore.sol:481:    JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_tierId);
JBTiered721DelegateStore.sol:555:    JBStored721Tier memory _storedTier;
JBTiered721DelegateStore.sol:628:  function recordAddTiers(JB721TierParams[] memory _tiersToAdd)
JBTiered721DelegateStore.sol:631:    returns (uint256[] memory tierIds)
JBTiered721DelegateStore.sol:654:    JB721TierParams memory _tierToAdd;
JBTiered721DelegateStore.sol:657:    JBTiered721Flags memory _flags = _flagsOf[msg.sender];
JBTiered721DelegateStore.sol:717:        JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_currentSortIndex);
JBTiered721DelegateStore.sol:811:    returns (uint256[] memory tokenIds)
JBTiered721DelegateStore.sol:937:    JBStored721Tier memory _storedTier;
JBTiered721DelegateStore.sol:948:    JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_currentSortIndex);
JBTiered721DelegateStore.sol:1016:  ) external override returns (uint256[] memory tokenIds, uint256 leftoverAmount) {
JBTiered721DelegateStore.sol:1033:    JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_tierIds[0]);
JBTiered721DelegateStore.sol:1091:  function recordBurn(uint256[] memory _tokenIds) external override {
JBTiered721DelegateStore.sol:1184:    JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_currentSortIndex);
JBTiered721DelegateStore.sol:1227:    JBStored721Tier memory _storedTier
```

## [G-2] Use preincrement
`++i` costs less gas than `i++`, especially when it's used in for-loops (--i/i-- too) Saves 5 gas PER LOOP
```solidity
JBIpfsDecoder.sol:68:    for (uint256 i = 0; i < _length; i++) {
JBIpfsDecoder.sol:76:    for (uint256 i = 0; i < _input.length; i++) {
JBIpfsDecoder.sol:84:    for (uint256 i = 0; i < _indices.length; i++) {
```
## [G-3] Cache `.length` in loops
Reading array length at each iteration of the loop takes 6 gas (3 for mload and 3 to place memory_offset) in the stack.
Caching the array length in the stack saves around 3 gas per iteration.

```solidity
JBIpfsDecoder.sol:49:    for (uint256 i = 0; i < _source.length; ++i) {
JBIpfsDecoder.sol:76:    for (uint256 i = 0; i < _input.length; i++) {
JBIpfsDecoder.sol:84:    for (uint256 i = 0; i < _indices.length; i++) {
```

## [G-4] Donot use default values Explicit initialization with zero is not required for variable declaration of numTokens because `uints are 0` by default.removeing this will reduce contract size and save a bit of gas.

```solidity
JBIpfsDecoder.sol:49:    for (uint256 i = 0; i < _source.length; ++i) {
JBIpfsDecoder.sol:51:      for (uint256 j = 0; j < digitlength; ++j) {
JBIpfsDecoder.sol:68:    for (uint256 i = 0; i < _length; i++) {
JBIpfsDecoder.sol:76:    for (uint256 i = 0; i < _input.length; i++) {
JBIpfsDecoder.sol:84:    for (uint256 i = 0; i < _indices.length; i++) {
```

## [G-5] Use variable == false|0 -> !variable or variable ==  true -> variable
```solidity
JB721Delegate.sol:130:    if (_data.redemptionRate == 0) return (0, _data.memo, delegateAllocations);
JB721TieredGovernance.sol:279:    if (_from == _to || _amount == 0) return;
JBTiered721Delegate.sol:628:    if (_tokenId == 0) {
JBIpfsDecoder.sol:45:    if (_source.length == 0) return new string(0);
JBTiered721DelegateStore.sol:435:    if (_balance == 0) return 0;
JBTiered721DelegateStore.sol:648:    uint256 _startSortIndex = _currentMaxTierIdOf == 0 ? 0 : _firstSortIndexOf(msg.sender);
JBTiered721DelegateStore.sol:682:      if (_tierToAdd.initialQuantity == 0) revert NO_QUANTITY();
JBTiered721DelegateStore.sol:760:          else if (_next == 0 || _next > _currentMaxTierIdOf) {
JBTiered721DelegateStore.sol:980:    if (tierId == 0) leftoverAmount = _amount;
JBTiered721DelegateStore.sol:1053:      if (_storedTier.initialQuantity == 0) revert INVALID_TIER();
JBTiered721DelegateStore.sol:1230:    if (_storedTier.initialQuantity == 0 || _storedTier.reservedRate == 0) return 0;
JBTiered721DelegateStore.sol:1317:    if (index == 0) index = 1;
JBTiered721DelegateStore.sol:1331:    if (index == 0) index = maxTierIdOf[_nft];
```
## [G-06] Use `!= 0` instead of `> 0`
```solidity
JB721Delegate.sol:116:    if (_data.tokenCount > 0) revert UNEXPECTED_TOKEN_REDEEMED();
JBTiered721Delegate.sol:240:    if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);
JBIpfsDecoder.sol:57:      while (carry > 0) {
JBTiered721DelegateStore.sol:1254:    if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
```

## [G-7] `<x> += <y>` costs more gas than `<x> = <x> + <y>` 
```solidity
JBIpfsDecoder.sol:52:        carry += uint256(digits[j]) * 256;
JBTiered721DelegateStore.sol:354:      supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;
JBTiered721DelegateStore.sol:409:        units += _balance * _storedTierOf[_nft][_i].votingUnits;
JBTiered721DelegateStore.sol:506:      balance += tierBalanceOf[_nft][_owner][_i];
JBTiered721DelegateStore.sol:534:      weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
JBTiered721DelegateStore.sol:563:      weight +=
JBTiered721DelegateStore.sol:827:    numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

## [G-8] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
check mannulay and only use of addresses is used as a key

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.
```solidity
JB721TieredGovernance.sol:44:  mapping(address => mapping(uint256 => address)) internal _tierDelegation;
JB721TieredGovernance.sol:53:  mapping(address => mapping(uint256 => Checkpoints.History)) internal _delegateTierCheckpoints;
JBTiered721Delegate.sol:86:  mapping(address => uint256) public override creditsOf;
JBTiered721DelegateStore.sol:57:  mapping(address => mapping(uint256 => uint256)) internal _tierIdAfter;
JBTiered721DelegateStore.sol:66:  mapping(address => mapping(uint256 => address)) internal _reservedTokenBeneficiaryOf;
JBTiered721DelegateStore.sol:75:  mapping(address => mapping(uint256 => JBStored721Tier)) internal _storedTierOf;
JBTiered721DelegateStore.sol:83:  mapping(address => JBTiered721Flags) internal _flagsOf;
JBTiered721DelegateStore.sol:93:  mapping(address => mapping(uint256 => uint256)) internal _isTierRemoved;
JBTiered721DelegateStore.sol:104:  mapping(address => uint256) internal _trackedLastSortTierIdOf;
JBTiered721DelegateStore.sol:119:  mapping(address => uint256) public override maxTierIdOf;
JBTiered721DelegateStore.sol:129:  mapping(address => mapping(address => mapping(uint256 => uint256))) public override tierBalanceOf;
JBTiered721DelegateStore.sol:138:  mapping(address => mapping(uint256 => uint256)) public override numberOfReservesMintedFor;
JBTiered721DelegateStore.sol:147:  mapping(address => mapping(uint256 => uint256)) public override numberOfBurnedFor;
JBTiered721DelegateStore.sol:155:  mapping(address => address) public override defaultReservedTokenBeneficiaryOf;
JBTiered721DelegateStore.sol:164:  mapping(address => mapping(uint256 => address)) public override firstOwnerOf;
JBTiered721DelegateStore.sol:172:  mapping(address => string) public override baseUriOf;
JBTiered721DelegateStore.sol:180:  mapping(address => IJBTokenUriResolver) public override tokenUriResolverOf;
JBTiered721DelegateStore.sol:188:  mapping(address => string) public override contractUriOf;
JBTiered721DelegateStore.sol:197:  mapping(address => mapping(uint256 => bytes32)) public override encodedIPFSUriOf;
```

## [G-9] Use custom errors to save deployment and runtime costs in case of revert
Instead of using strings for error messages (e.g., require(msg.sender == owner, “unauthorized”)), you can use custom errors to reduce both deployment and runtime gas costs. In addition, they are very convenient as you can easily pass dynamic information to them.
```solidity
JBTiered721Delegate.sol:216:    require(address(this) != codeOrigin);
JBTiered721Delegate.sol:218:    require(address(store) == address(0));
```


## [G-10] `++i`/`i++` should be `unchecked{++i}`/`unchecked{++i}` when it is not possible for them to overflow, as is the case when used in `for`- and `while`-loops :
```solidity
JBIpfsDecoder.sol:49:    for (uint256 i = 0; i < _source.length; ++i) {
JBIpfsDecoder.sol:51:      for (uint256 j = 0; j < digitlength; ++j) {
```
