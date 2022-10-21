## 1. Using `bool`s for storage incurs overhead


// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and 
// pointer aliasing, and it cannot be disabled.

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27

Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from `false` to `true`, after having been `true` in the past

```
543		bool _expectMintFromExtraFunds;
546		bool _dontOverspend;
616		bool _expectMint
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol
```
1015		bool _isManualMint
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

## 2. Not using the named return variables when a function returns, wastes deployment gas

```
39		return store.votingUnitsOf(address(this), _account);
```	
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol
```
220		return
221			controller.launchFundingCyclesFor(
222			_projectId,
223			_launchFundingCyclesData.data,
224			_launchFundingCyclesData.metadata,
225			_launchFundingCyclesData.mustStartAtOrAfter,
226			_launchFundingCyclesData.groupedSplits,
227			_launchFundingCyclesData.fundAccessConstraints,
228			_launchFundingCyclesData.terminals,
229			_launchFundingCyclesData.memo
230		);
...
246		return
247			controller.reconfigureFundingCyclesOf(
248			_projectId,
249			_reconfigureFundingCyclesData.data,
250			_reconfigureFundingCyclesData.metadata,
251			_reconfigureFundingCyclesData.mustStartAtOrAfter,
252			_reconfigureFundingCyclesData.groupedSplits,
253			_reconfigureFundingCyclesData.fundAccessConstraints,
254			_reconfigureFundingCyclesData.memo
255		);
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol
```
80		return _tierDelegation[_account][_tier];
91		return _delegateTierCheckpoints[_account][_tier].latest();
107		return _delegateTierCheckpoints[_account][_tier].getAtBlock(_blockNumber);
117		return _totalTierCheckpoints[_tier].latest();
134		return _totalTierCheckpoints[_tier].getAtBlock(_blockNumber);
200		return store.tierVotingUnitsOf(address(this), _account, _tierId);
323		return a + b;
327		return a - b;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol
```
108		return _owners[_tokenId];
124		return store.balanceOf(address(this), _owner);
...
149		return
150			JBIpfsDecoder.decode(
151			store.baseUriOf(address(this)),
152			store.encodedTierIPFSUriOf(address(this), _tokenId)
153		);
...
163		return store.contractUriOf(address(this));
702		return store.redemptionWeightOf(address(this), _tokenIds);
712		return store.totalRedemptionWeight(address(this));
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol
```
438		return _balance * _storedTierOf[_nft][_tierId].votingUnits;
456		return encodedIPFSUriOf[_nft][tierIdOfToken(_tokenId)];
468		return _flagsOf[_nft];
483		return _bitmapWord.isTierIdRemoved(_tierId);
587		return uint256(uint16(_tokenId));
1230		if (_storedTier.initialQuantity == 0 || _storedTier.reservedRate == 0) return 0;
1233		if (_storedTier.initialQuantity == _storedTier.remainingQuantity) return 1;
1240		return 0;
1303		return _index + 1;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol
```
146		return (_base, _data.memo, delegateAllocations);
...
149		return (
150			PRBMath.mulDiv(
151				_base,
152				_data.redemptionRate +
153					PRBMath.mulDiv(
154						_redemptionWeight,
155						JBConstants.MAX_REDEMPTION_RATE - _data.redemptionRate,
156						_total
157					),
158				JBConstants.MAX_REDEMPTION_RATE
159			),
160			_data.memo,
161			delegateAllocations
162		);
...
325		return 0;
335		return 0;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol
```
22		return JBBitmapWord({currentWord: self[_depth], currentDepth: _depth});
30		return (self.currentWord >> (_index % 256)) & 1 == 1;
43		return isTierIdRemoved(JBBitmapWord({currentWord: self[_depth], currentDepth: _depth}), _index);
64		return _retrieveDepth(_index) != self.currentDepth;
74		return _index / 256;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol
```
34		return string(abi.encodePacked(_baseUri, ipfsHash));
63		return string(_toAlphabet(_reverse(_truncate(digits, digitlength))));
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol


## 3. `public` functions not called by the contract should be declared `external` instead


Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents’ functions and change the visibility from `external` to `public` and can save gas by doing so

```
123		function balanceOf(address _owner) public view override returns (uint256 balance) {
138		function tokenURI(uint256 _tokenId) public view override returns (string memory) {
175		function supportsInterface(bytes4 _interfaceId) public view override returns (bool) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol
```
499		function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) {
523		function redemptionWeightOf(address _nft, uint256[] calldata _tokenIds)
550		function totalRedemptionWeight(address _nft) public view override returns (uint256 weight) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol
```
178		function supportsInterface(bytes4 _interfaceId)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol

## 4. Constructor parameters should be avoided when possible

Constructor parameters are expensive. The contract deployment will be cheaper in gas if they are hard coded instead of using constructor parameters.

```
51		globalGovernance = _globalGovernance;
52		tieredGovernance = _tieredGovernance;
53		noGovernance = _noGovernance;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol
```
52		controller = _controller;
53		delegateDeployer = _delegateDeployer;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol
```
211		projectId = _projectId;
212		directory = _directory;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol


## 5. Functions guaranteed to revert when called by normal users can be marked `payable`


If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are `CALLVALUE`(2),`DUP1`(3),`ISZERO`(3),`PUSH2`(3),`JUMPI`(10),`PUSH1`(3),`DUP1`(3),`REVERT`(0),`JUMPDEST`(1),`POP`(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost


```
370		function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {
386		function setBaseUri(string memory _baseUri) external override onlyOwner {
402		function setContractUri(string calldata _contractUri) external override onlyOwner {
418		function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {
480		function mintFor(uint16[] memory _tierIds, address _beneficiary)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol


## 6. Multiple `address`/`ID` mappings can be combined into a single `mapping `of an `address`/`ID` to a `struct`, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key’s keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

```
44		mapping(address => mapping(uint256 => address)) internal _tierDelegation;
53		mapping(address => mapping(uint256 => Checkpoints.History)) internal _delegateTierCheckpoints;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol
```
57		mapping(address => mapping(uint256 => uint256)) internal _tierIdAfter;
66		mapping(address => mapping(uint256 => address)) internal _reservedTokenBeneficiaryOf;
75		mapping(address => mapping(uint256 => JBStored721Tier)) internal _storedTierOf;
83		mapping(address => JBTiered721Flags) internal _flagsOf;
93		mapping(address => mapping(uint256 => uint256)) internal _isTierRemoved;
104		mapping(address => uint256) internal _trackedLastSortTierIdOf;
...
119		mapping(address => uint256) public override maxTierIdOf;
129		mapping(address => mapping(address => mapping(uint256 => uint256))) public override tierBalanceOf;
138		mapping(address => mapping(uint256 => uint256)) public override numberOfReservesMintedFor;
147		mapping(address => mapping(uint256 => uint256)) public override numberOfBurnedFor;
155		mapping(address => address) public override defaultReservedTokenBeneficiaryOf;
164		mapping(address => mapping(uint256 => address)) public override firstOwnerOf;
172		mapping(address => string) public override baseUriOf;
180		mapping(address => IJBTokenUriResolver) public override tokenUriResolverOf;
188		mapping(address => string) public override contractUriOf;
197		mapping(address => mapping(uint256 => bytes32)) public override encodedIPFSUriOf;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol









