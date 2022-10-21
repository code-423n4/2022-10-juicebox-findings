

### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gsset (20000 gas)
#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::54 => IJBTiered721DelegateStore public override store;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::60 => IJBFundingCycleStore public override fundingCycleStore;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::66 => IJBPrices public override prices;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::72 => uint256 public override pricingCurrency;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::78 => uint256 public override pricingDecimals;
```



### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor, avoids a separate Gsset (20000 gas). 
Reads of the variables are also cheaper
#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::48 => address public override codeOrigin;
```



### [G03] `<array>.length` should not be looked up in every loop of a `for` loop

#### Impact
Even memory arrays incur the overhead of bit tests and bit shifts to 
calculate the array length. Storage array length checks incur an extra 
Gwarmaccess (100 gas) PER-LOOP.
#### Findings:
```

juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```



### [G04] `++i/i++` should be `unchecked{++i}`/`unchecked{++i}` when it is not possible for them to overflow, as is the case when used in `for` and `while` loops


#### Findings:
```
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```



### [G05] Not using the named return variables when a function returns, wastes deployment gas


#### Findings:
```
juice-nft-rewards/contracts/JB721TieredGovernance.sol::80 => return _tierDelegation[_account][_tier];
juice-nft-rewards/contracts/JB721TieredGovernance.sol::91 => return _delegateTierCheckpoints[_account][_tier].latest();
juice-nft-rewards/contracts/JB721TieredGovernance.sol::107 => return _delegateTierCheckpoints[_account][_tier].getAtBlock(_blockNumber);
juice-nft-rewards/contracts/JB721TieredGovernance.sol::117 => return _totalTierCheckpoints[_tier].latest();
juice-nft-rewards/contracts/JB721TieredGovernance.sol::134 => return _totalTierCheckpoints[_tier].getAtBlock(_blockNumber);
juice-nft-rewards/contracts/JBTiered721Delegate.sol::108 => return _owners[_tokenId];
juice-nft-rewards/contracts/JBTiered721DelegateProjectDeployer.sol::134 => return _launchFundingCyclesFor(_projectId, _launchFundingCyclesData);
juice-nft-rewards/contracts/JBTiered721DelegateProjectDeployer.sol::177 => return _reconfigureFundingCyclesOf(_projectId, _reconfigureFundingCyclesData);
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::377 => return _numberOfReservedTokensOutstandingFor(_nft, _tierId, _storedTierOf[_nft][_tierId]);
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::468 => return _flagsOf[_nft];
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::483 => return _bitmapWord.isTierIdRemoved(_tierId);
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::610 => return _storedReservedTokenBeneficiaryOfTier;
```



### [G06] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement


#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::240 => if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1254 => if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::116 => if (_data.tokenCount > 0) revert UNEXPECTED_TOKEN_REDEEMED();
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::57 => while (carry > 0) {
```



### [G07] It costs more gas to initialize variables to zero than to let the default of zero be applied


#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::588 => } else if (_credits != 0) creditsOf[_data.beneficiary] = 0;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::589 => } else if (_credits != 0) creditsOf[_data.beneficiary] = 0;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::349 => for (uint256 _i = _maxTierId; _i != 0; ) {
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::403 => for (uint256 _i = _maxTierId; _i != 0; ) {
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::504 => for (uint256 _i = _maxTierId; _i != 0; ) {
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::757 => _currentSortIndex = 0;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::768 => _currentSortIndex = 0;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::772 => _trackedLastSortTierIdOf[msg.sender] = 0;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::959 => if (_storedTier.contributionFloor > _amount) _currentSortIndex = 0;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1198 => } else if (_tierIdAfter[_nft][_previous] != 0) _tierIdAfter[_nft][_previous] = 0;
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::47 => digits[0] = 0;
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```



### [G08] `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too)


#### Findings:
```
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```



### [G09] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::370 => function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::386 => function setBaseUri(string memory _baseUri) external override onlyOwner {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::402 => function setContractUri(string calldata _contractUri) external override onlyOwner {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::418 => function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {
```



### [G10] Use `calldata` instead of `memory` for function parameters

#### Impact
Use calldata instead of memory for function parameters. Having function arguments use calldata instead of memory can save gas.
#### Findings:
```
juice-nft-rewards/contracts/JB721TieredGovernance.sol::147 => function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)
juice-nft-rewards/contracts/JBTiered721Delegate.sol::264 => function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)
juice-nft-rewards/contracts/JBTiered721Delegate.sol::290 => function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)
juice-nft-rewards/contracts/JBTiered721Delegate.sol::386 => function setBaseUri(string memory _baseUri) external override onlyOwner {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::480 => function mintFor(uint16[] memory _tierIds, address _beneficiary)
juice-nft-rewards/contracts/JBTiered721Delegate.sol::598 => function _didBurn(uint256[] memory _tokenIds) internal override {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::695 => function _redemptionWeightOf(uint256[] memory _tokenIds)
juice-nft-rewards/contracts/JBTiered721DelegateProjectDeployer.sol::191 => function _launchProjectFor(address _owner, JBLaunchProjectData memory _launchProjectData)
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::628 => function recordAddTiers(JB721TierParams[] memory _tiersToAdd)
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1091 => function recordBurn(uint256[] memory _tokenIds) external override {
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::263 => (, uint256[] memory _decodedTokenIds) = abi.decode(_data.metadata, (bytes4, uint256[]));
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::311 => function _didBurn(uint256[] memory _tokenIds) internal virtual {
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::323 => function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {
juice-nft-rewards/contracts/libraries/JBBitmap.sol::29 => function isTierIdRemoved(JBBitmapWord memory self, uint256 _index) internal pure returns (bool) {
juice-nft-rewards/contracts/libraries/JBBitmap.sol::59 => function refreshBitmapNeeded(JBBitmapWord memory self, uint256 _index)
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::22 => function decode(string memory _baseUri, bytes32 _hexString)
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::44 => function _toBase58(bytes memory _source) private pure returns (string memory) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::66 => function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::74 => function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::82 => function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```





### [G11] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
Saves a storage slot for the mapping. Depending on the circumstances 
and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires
 both values and they both fit in the same storage slot
#### Findings:
```
juice-nft-rewards/contracts/JB721TieredGovernance.sol::44 => mapping(address => mapping(uint256 => address)) internal _tierDelegation;
juice-nft-rewards/contracts/JB721TieredGovernance.sol::53 => mapping(address => mapping(uint256 => Checkpoints.History)) internal _delegateTierCheckpoints;


juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::57 => mapping(address => mapping(uint256 => uint256)) internal _tierIdAfter;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::66 => mapping(address => mapping(uint256 => address)) internal _reservedTokenBeneficiaryOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::75 => mapping(address => mapping(uint256 => JBStored721Tier)) internal _storedTierOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::93 => mapping(address => mapping(uint256 => uint256)) internal _isTierRemoved;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::119 => mapping(address => uint256) public override maxTierIdOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::129 => mapping(address => mapping(address => mapping(uint256 => uint256))) public override tierBalanceOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::138 => mapping(address => mapping(uint256 => uint256)) public override numberOfReservesMintedFor;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::147 => mapping(address => mapping(uint256 => uint256)) public override numberOfBurnedFor;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::155 => mapping(address => address) public override defaultReservedTokenBeneficiaryOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::164 => mapping(address => mapping(uint256 => address)) public override firstOwnerOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::172 => mapping(address => string) public override baseUriOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::180 => mapping(address => IJBTokenUriResolver) public override tokenUriResolverOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::188 => mapping(address => string) public override contractUriOf;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::197 => mapping(address => mapping(uint256 => bytes32)) public override encodedIPFSUriOf;
```


### [G12] Using `bools` for storage incurs overhead


#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::543 => bool _expectMintFromExtraFunds;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::546 => bool _dontOverspend;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::555 => bool _dontMint;
```


### [G13] Remove or replace unused state variables

#### Impact
Saves a storage slot. If the variable is assigned a non-zero value, 
saves Gsset (20000 gas). If it’s assigned a zero value, saves Gsreset 
(2900 gas). If the variable remains unassigned, there is no gas savings 
unless the variable is public, in which case the 
compiler-generated non-payable getter deployment cost is saved. If the 
state variable is overriding an interface’s public function, mark the 
variable as constant or immutable so that it does not use a storage slot
#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::321 => function adjustTiers(JB721TierParams[] calldata _tiersToAdd, uint256[] calldata _tierIdsToRemove)
juice-nft-rewards/contracts/JBTiered721Delegate.sol::349 => uint256[] memory _tierIdsAdded = store.recordAddTiers(_tiersToAdd);
juice-nft-rewards/contracts/JBTiered721Delegate.sol::448 => uint256[] memory _tokenIds = store.recordMintReservesFor(_tierId, _count);
juice-nft-rewards/contracts/JBTiered721Delegate.sol::480 => function mintFor(uint16[] memory _tierIds, address _beneficiary)
juice-nft-rewards/contracts/JBTiered721Delegate.sol::484 => returns (uint256[] memory tokenIds)
juice-nft-rewards/contracts/JBTiered721Delegate.sol::558 => uint16[] memory _tierIdsToMint;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::563 => (bytes32, bytes4, bool, bool, bool, uint16[])
juice-nft-rewards/contracts/JBTiered721Delegate.sol::598 => function _didBurn(uint256[] memory _tokenIds) internal override {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::652 => uint16[] memory _mintTierIds,
juice-nft-rewards/contracts/JBTiered721Delegate.sol::656 => uint256[] memory _tokenIds;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::695 => function _redemptionWeightOf(uint256[] memory _tokenIds)
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::523 => function redemptionWeightOf(address _nft, uint256[] calldata _tokenIds)
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::631 => returns (uint256[] memory tierIds)
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::643 => tierIds = new uint256[](_numberOfNewTiers);
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::811 => returns (uint256[] memory tokenIds)
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::830 => tokenIds = new uint256[](_count);
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::891 => function recordRemoveTierIds(uint256[] calldata _tierIds) external override {
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1014 => uint16[] calldata _tierIds,
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1016 => ) external override returns (uint256[] memory tokenIds, uint256 leftoverAmount) {
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1030 => tokenIds = new uint256[](_numberOfTiers);
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1091 => function recordBurn(uint256[] memory _tokenIds) external override {
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::133 => (, uint256[] memory _decodedTokenIds) = abi.decode(_data.metadata, (bytes4, uint256[]));
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::263 => (, uint256[] memory _decodedTokenIds) = abi.decode(_data.metadata, (bytes4, uint256[]));
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::311 => function _didBurn(uint256[] memory _tokenIds) internal virtual {
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::323 => function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::46 => uint8[] memory digits = new uint8[](46); // hash size with the prefix
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::66 => function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::67 => uint8[] memory output = new uint8[](_length);
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::74 => function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::75 => uint8[] memory output = new uint8[](_input.length);
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::82 => function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```





### [G14] Optimize names to save gas

#### Impact
public/external function names and public member variable names can be optimized to save gas. [See this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9)
 link for an example of how it works. Below are the interfaces/abstract 
contracts that can be optimized so that the most frequently-called 
functions use the least amount of gas possible during method lookup. 
#### Findings:
```
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::31 => abstract contract JB721Delegate is
```



