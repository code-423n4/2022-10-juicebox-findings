## [G-001] Don't Initialize Variables with Default Value

### Description:

Uninitialized variables are assigned with the types default value.

### Findings:

Total:5

[JBIpfsDecoder.sol#L49](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49)

```
49:    for (uint256 i = 0; i < _source.length; ++i) {
```

[JBIpfsDecoder.sol#L51](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L51)

```
51:    for (uint256 j = 0; j < digitlength; ++j) {
```

[JBIpfsDecoder.sol#L68](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68)

```
68:    for (uint256 i = 0; i < _length; i++) {
```

[JBIpfsDecoder.sol#L76](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)

```
76:    for (uint256 i = 0; i < _input.length; i++) {
```

[JBIpfsDecoder.sol#L84](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)

```
84:    for (uint256 i = 0; i < _indices.length; i++) {
```

## [G-002] Functions guaranteed to revert when called by normal users can be marked payable

### Description:

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Findings:

Total:4

[JBTiered721Delegate.sol#L370](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L370)

```
370:    function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {
```

[JBTiered721Delegate.sol#L386](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L386)

```
386:    function setBaseUri(string memory _baseUri) external override onlyOwner {
```

[JBTiered721Delegate.sol#L402](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L402)

```
402:    function setContractUri(string calldata _contractUri) external override onlyOwner {
```

[JBTiered721Delegate.sol#L418](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L418)

```
418:    function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {
```

## [G-003] Use Shift Right/Left instead of Division/Multiplication if possible

### Description:

A division/multiplication by any number `x` being a power of 2 can be calculated by shifting `log2(x)` to the right/left.

### Findings:

Total:2

[JBBitmap.sol#L74](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L74)

```
74:    return _index / 256;
```

[JBIpfsDecoder.sol#L52](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L52)

```
52:    carry += uint256(digits[j]) * 256;
```

## [G-004] Unnecessary checked arithmetic in for loop

### Description:

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30~40 gas per loop.

### Findings:

Total:6

[JBTiered721DelegateStore.sol#L245](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L245)

```
245:    _tiers[_numberOfIncludedTiers++] = JB721Tier({
```

[JBTiered721DelegateStore.sol#L1108](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1108)

```
1108:    _storedTierOf[msg.sender][_tierId].remainingQuantity++;
```

[JBIpfsDecoder.sol#L59](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L59)

```
59:    digitlength++;
```

[JBIpfsDecoder.sol#L68](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68)

```
68:    for (uint256 i = 0; i < _length; i++) {
```

[JBIpfsDecoder.sol#L76](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)

```
76:    for (uint256 i = 0; i < _input.length; i++) {
```

[JBIpfsDecoder.sol#L84](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)

```
84:    for (uint256 i = 0; i < _indices.length; i++) {
```

## [G-005] Use Prefix Increment instead of Postfix Increment if possible

### Description:

`++i` costs less gas than `i++`, especially when it's used in for-loops (`--i/i--`) Saves 5 gas per loop.

### Findings:

Total:6

[JBTiered721DelegateStore.sol#L245](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L245)

```
245:    _tiers[_numberOfIncludedTiers++] = JB721Tier({
```

[JBTiered721DelegateStore.sol#L1108](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1108)

```
1108:    _storedTierOf[msg.sender][_tierId].remainingQuantity++;
```

[JBIpfsDecoder.sol#L59](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L59)

```
59:    digitlength++;
```

[JBIpfsDecoder.sol#L68](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68)

```
68:    for (uint256 i = 0; i < _length; i++) {
```

[JBIpfsDecoder.sol#L76](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)

```
76:    for (uint256 i = 0; i < _input.length; i++) {
```

[JBIpfsDecoder.sol#L84](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)

```
84:    for (uint256 i = 0; i < _indices.length; i++) {
```

## [G-006] Not using the named return variables when a function returns, wastes deployment gas

### Description:

It is not necessary to have both a named return and a return statement.

### Findings:

Total:25

[JB721GlobalGovernance.sol#L37](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L37)

```
37:    returns (uint256 units)
```

[JBTiered721Delegate.sol#L123](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L123)

```
123:    function balanceOf(address _owner) public view override returns (uint256 balance) {
```

[JBTiered721Delegate.sol#L138](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L138)

```
138:    function tokenURI(uint256 _tokenId) public view override returns (string memory) {
```

[JBTiered721Delegate.sol#L162](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L162)

```
162:    function contractURI() external view override returns (string memory) {
```

[JBTiered721Delegate.sol#L617](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L617)

```
617:    ) internal returns (uint256 leftoverAmount) {
```

[JBTiered721Delegate.sol#L654](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L654)

```
654:    ) internal returns (uint256 leftoverAmount) {
```

[JBTiered721DelegateDeployer.sol#L115](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L115)

```
115:    function _clone(address _targetAddress) internal returns (address _out) {
```

[JBTiered721DelegateProjectDeployer.sol#L74](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L74)

```
74:    ) external override returns (uint256 projectId) {
```

[JBTiered721DelegateProjectDeployer.sol#L119](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L119)

```
119:    returns (uint256 configuration)
```

[JBTiered721DelegateProjectDeployer.sol#L162](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L162)

```
162:    returns (uint256 configuration)
```

[JBTiered721DelegateStore.sol#L279](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L279)

```
279:    function tier(address _nft, uint256 _id) external view override returns (JB721Tier memory) {
```

[JBTiered721DelegateStore.sol#L311](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L311)

```
311:    returns (JB721Tier memory)
```

[JBTiered721DelegateStore.sol#L342](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L342)

```
342:    function totalSupply(address _nft) external view override returns (uint256 supply) {
```

[JBTiered721DelegateStore.sol#L394](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L394)

```
394:    returns (uint256 units)
```

[JBTiered721DelegateStore.sol#L467](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L467)

```
467:    function flagsOf(address _nft) external view override returns (JBTiered721Flags memory) {
```

[JBTiered721DelegateStore.sol#L499](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L499)

```
499:    function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) {
```

[JBTiered721DelegateStore.sol#L527](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L527)

```
527:    returns (uint256 weight)
```

[JBTiered721DelegateStore.sol#L550](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L550)

```
550:    function totalRedemptionWeight(address _nft) public view override returns (uint256 weight) {
```

[JBTiered721DelegateStore.sol#L1273](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1273)

```
1273:    returns (uint256 tokenId)
```

[JBTiered721DelegateStore.sol#L1314](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1314)

```
1314:    function _firstSortIndexOf(address _nft) internal view returns (uint256 index) {
```

[JBTiered721DelegateStore.sol#L1328](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1328)

```
1328:    function _lastSortIndexOf(address _nft) internal view returns (uint256 index) {
```

[JBBitmap.sol#L18](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L18)

```
18:    returns (JBBitmapWord memory)
```

[JBIpfsDecoder.sol#L25](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L25)

```
25:    returns (string memory)
```

[JBIpfsDecoder.sol#L44](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L44)

```
44:    function _toBase58(bytes memory _source) private pure returns (string memory) {
```

[JBIpfsDecoder.sol#L82](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L82)

```
82:    function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```

## [G-007] Use Assembly to check for address(0)

### Description:

Saves 6 gas per instance if using assemlby to check for address(0)

### Findings:

Total:11

[JB721TieredGovernance.sol#L282](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L282)

```
282:    if (_from != address(0)) {
```

[JB721TieredGovernance.sol#L291](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L291)

```
291:    if (_to != address(0)) {
```

[JBTiered721Delegate.sol#L105](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L105)

```
105:    if (_storedFirstOwner != address(0)) return _storedFirstOwner;
```

[JBTiered721Delegate.sol#L146](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L146)

```
146:    if (address(_resolver) != address(0)) return _resolver.getUri(_tokenId);
```

[JBTiered721Delegate.sol#L729](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L729)

```
729:    if (_from != address(0)) {
```

[JBTiered721Delegate.sol#L736](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L736)

```
736:    _to != address(0) &&
```

[JBTiered721DelegateDeployer.sol#L99](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L99)

```
99:    if (_deployTiered721DelegateData.owner != address(0))
```

[JBTiered721DelegateStore.sol#L609](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L609)

```
609:    if (_storedReservedTokenBeneficiaryOfTier != address(0))
```

[JBTiered721DelegateStore.sol#L700](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L700)

```
700:    _tierToAdd.reservedTokenBeneficiary != address(0) &&
```

[JBTiered721DelegateStore.sol#L872](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L872)

```
872:    if (_from != address(0))
```

[JBTiered721DelegateStore.sol#L877](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L877)

```
877:    if (_to != address(0)) {
```

## [G-008] Using storage instead of memory for structs/arrays saves gas

### Description:

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Goldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read.

### Findings:

Total:21

[JBTiered721Delegate.sol#L273](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L273)

```
273:    JBTiered721MintReservesForTiersData memory _data = _mintReservesForTiersData[_i];
```

[JBTiered721Delegate.sol#L300](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L300)

```
300:    JBTiered721MintForTiersData memory _data = _mintForTiersData[_i];
```

[JBTiered721Delegate.sol#L349](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L349)

```
349:    uint256[] memory _tierIdsAdded = store.recordAddTiers(_tiersToAdd);
```

[JBTiered721Delegate.sol#L438](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L438)

```
438:    JBFundingCycle memory _fundingCycle = fundingCycleStore.currentOf(projectId);
```

[JBTiered721Delegate.sol#L448](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L448)

```
448:    uint256[] memory _tokenIds = store.recordMintReservesFor(_tierId, _count);
```

[JBTiered721Delegate.sol#L733](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L733)

```
733:    JBFundingCycle memory _fundingCycle = fundingCycleStore.currentOf(projectId);
```

[JBTiered721Delegate.sol#L765](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L765)

```
765:    JB721Tier memory _tier = store.tierOfTokenId(address(this), _tokenId);
```

[JBTiered721DelegateStore.sol#L234](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L234)

```
234:    JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_currentSortIndex);
```

[JBTiered721DelegateStore.sol#L281](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L281)

```
281:    JBStored721Tier memory _storedTier = _storedTierOf[_nft][_id];
```

[JBTiered721DelegateStore.sol#L317](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L317)

```
317:    JBStored721Tier memory _storedTier = _storedTierOf[_nft][_tierId];
```

[JBTiered721DelegateStore.sol#L481](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L481)

```
481:    JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_tierId);
```

[JBTiered721DelegateStore.sol#L657](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L657)

```
657:    JBTiered721Flags memory _flags = _flagsOf[msg.sender];
```

[JBTiered721DelegateStore.sol#L717](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L717)

```
717:    JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_currentSortIndex);
```

[JBTiered721DelegateStore.sol#L948](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L948)

```
948:    JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_currentSortIndex);
```

[JBTiered721DelegateStore.sol#L1033](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1033)

```
1033:    JBBitmapWord memory _bitmapWord = _isTierRemoved[msg.sender].readId(_tierIds[0]);
```

[JBTiered721DelegateStore.sol#L1184](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1184)

```
1184:    JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_currentSortIndex);
```

[JBIpfsDecoder.sol#L28](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L28)

```
28:    bytes memory completeHexString = abi.encodePacked(bytes2(0x1220), _hexString);
```

[JBIpfsDecoder.sol#L31](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L31)

```
31:    string memory ipfsHash = _toBase58(completeHexString);
```

[JBIpfsDecoder.sol#L67](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L67)

```
67:    uint8[] memory output = new uint8[](_length);
```

[JBIpfsDecoder.sol#L75](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L75)

```
75:    uint8[] memory output = new uint8[](_input.length);
```

[JBIpfsDecoder.sol#L83](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L83)

```
83:    bytes memory output = new bytes(_indices.length);
```

## [G-009] internal function only called once can be inlined to save gas

### Description:

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

### Findings:

Total:6

[JBTiered721Delegate.sol#L711](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L711)

```
711:    function _totalRedemptionWeight() internal view virtual override returns (uint256) {
```

[JBTiered721DelegateDeployer.sol#L115](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L115)

```
115:    function _clone(address _targetAddress) internal returns (address _out) {
```

[JB721Delegate.sol#L301](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L301)

```
301:    function _processPayment(JBDidPayData calldata _data) internal virtual {
```

[JB721Delegate.sol#L311](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L311)

```
311:    function _didBurn(uint256[] memory _tokenIds) internal virtual {
```

[JB721Delegate.sol#L323](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L323)

```
323:    function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {
```

[JB721Delegate.sol#L334](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L334)

```
334:    function _totalRedemptionWeight() internal view virtual returns (uint256) {
```

## [G-010] internal function not called by the contract should be removed to save deployment gas

### Description:

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword.

### Findings:

Total:2

[JBTiered721Delegate.sol#L524](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L524)

```
524:    function _processPayment(JBDidPayData calldata _data) internal override {
```

[JBTiered721Delegate.sol#L598](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L598)

```
598:    function _didBurn(uint256[] memory _tokenIds) internal override {
```