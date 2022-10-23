## [NC-001] Adding a return statement to a function that returns a value is redundant

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

## [L-001] Events not emitted for important state changes / Missing event for critical parameter changes

### Description:

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

### Findings:

Total:6

[JB721TieredGovernance.sol#L147](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L147)

```
147:    function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)
```

[JB721TieredGovernance.sol#L177](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L177)

```
177:    function setTierDelegate(address _delegatee, uint256 _tierId) public virtual override {
```

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
