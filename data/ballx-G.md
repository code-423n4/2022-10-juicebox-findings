# c4udit Report

## Files analyzed
- contracts/JB721GlobalGovernance.sol
- contracts/JB721TieredGovernance.sol
- contracts/JBTiered721Delegate.sol
- contracts/JBTiered721DelegateDeployer.sol
- contracts/JBTiered721DelegateProjectDeployer.sol
- contracts/JBTiered721DelegateStore.sol
- contracts/abstract/ERC721.sol
- contracts/abstract/JB721Delegate.sol
- contracts/abstract/Votes.sol
- contracts/enums/JB721GovernanceType.sol
- contracts/forge-test/Deployer_Unit.t.sol
- contracts/forge-test/E2E.t.sol
- contracts/forge-test/NFTReward_Unit.t.sol
- contracts/forge-test/governance/JB721GlobalGovernance.t.sol
- contracts/forge-test/governance/JB721TieredGovernance.t.sol
- contracts/forge-test/utils/AccessJBLib.sol
- contracts/forge-test/utils/TestBaseWorkflow.sol
- contracts/interfaces/IJB721Delegate.sol
- contracts/interfaces/IJB721TieredGovernance.sol
- contracts/interfaces/IJBTiered721Delegate.sol
- contracts/interfaces/IJBTiered721DelegateDeployer.sol
- contracts/interfaces/IJBTiered721DelegateProjectDeployer.sol
- contracts/interfaces/IJBTiered721DelegateStore.sol
- contracts/libraries/JBBitmap.sol
- contracts/libraries/JBIpfsDecoder.sol
- contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol
- contracts/scripts/Deploy.s.sol
- contracts/scripts/LaunchProjectFor.s.sol
- contracts/structs/JB721PricingParams.sol
- contracts/structs/JB721Tier.sol
- contracts/structs/JB721TierParams.sol
- contracts/structs/JBBitmapWord.sol
- contracts/structs/JBDeployTiered721DelegateData.sol
- contracts/structs/JBLaunchFundingCyclesData.sol
- contracts/structs/JBLaunchProjectData.sol
- contracts/structs/JBReconfigureFundingCyclesData.sol
- contracts/structs/JBStored721Tier.sol
- contracts/structs/JBTiered721Flags.sol
- contracts/structs/JBTiered721FundingCycleMetadata.sol
- contracts/structs/JBTiered721MintForTiersData.sol
- contracts/structs/JBTiered721MintReservesForTiersData.sol
- contracts/structs/JBTiered721SetTierDelegatesData.sol

## Issues found

### Don't Initialize Variables with Default Value

#### Impact
Issue Information: [G001](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g001---dont-initialize-variables-with-default-value)

#### Findings:
```
contracts/forge-test/E2E.t.sol::184 => for (uint256 i = 0; i < 5; i++) {
contracts/libraries/JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {
contracts/libraries/JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {
contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Cache Array Length Outside of Loop

#### Impact
Issue Information: [G002](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g002---cache-array-length-outside-of-loop)

#### Findings:
```
contracts/JB721TieredGovernance.sol::153 => uint256 _numberOfTierDelegates = _setTierDelegatesData.length;
contracts/JBTiered721Delegate.sol::230 => if (bytes(_baseUri).length != 0) _store.recordSetBaseUri(_baseUri);
contracts/JBTiered721Delegate.sol::233 => if (bytes(_contractUri).length != 0) _store.recordSetContractUri(_contractUri);
contracts/JBTiered721Delegate.sol::240 => if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);
contracts/JBTiered721Delegate.sol::269 => uint256 _numberOfTiers = _mintReservesForTiersData.length;
contracts/JBTiered721Delegate.sol::296 => uint256 _numberOfBeneficiaries = _mintForTiersData.length;
contracts/JBTiered721Delegate.sol::327 => uint256 _numberOfTiersToAdd = _tiersToAdd.length;
contracts/JBTiered721Delegate.sol::330 => uint256 _numberOfTiersToRemove = _tierIdsToRemove.length;
contracts/JBTiered721Delegate.sol::494 => uint256 _numberOfTokens = _tierIds.length;
contracts/JBTiered721Delegate.sol::551 => _data.metadata.length > 36 &&
contracts/JBTiered721Delegate.sol::570 => if (_tierIdsToMint.length != 0)
contracts/JBTiered721Delegate.sol::666 => uint256 _mintsLength = _tokenIds.length;
contracts/JBTiered721DelegateDeployer.sol::123 => // Shift the length to the length placeholder, in the constructor
contracts/JBTiered721DelegateDeployer.sol::126 => // Insert the length in the correct sport (after the PUSH3 / 0x62)
contracts/JBTiered721DelegateStore.sol::221 => // Initialize an array with the appropriate length.
contracts/JBTiered721DelegateStore.sol::530 => uint256 _numberOfTokenIds = _tokenIds.length;
contracts/JBTiered721DelegateStore.sol::634 => uint256 _numberOfNewTiers = _tiersToAdd.length;
contracts/JBTiered721DelegateStore.sol::642 => // Initialize an array with the appropriate length.
contracts/JBTiered721DelegateStore.sol::829 => // Initialize an array with the appropriate length.
contracts/JBTiered721DelegateStore.sol::893 => uint256 _numTiers = _tierIds.length;
contracts/JBTiered721DelegateStore.sol::1021 => uint256 _numberOfTiers = _tierIds.length;
contracts/JBTiered721DelegateStore.sol::1029 => // Initialize an array with the appropriate length.
contracts/JBTiered721DelegateStore.sol::1093 => uint256 _numberOfTokenIds = _tokenIds.length;
contracts/abstract/ERC721.sol::112 => return bytes(baseURI).length > 0 ? string(abi.encodePacked(baseURI, tokenId.toString())) : '';
contracts/abstract/ERC721.sol::175 => //solhint-disable-next-line max-line-length
contracts/abstract/ERC721.sol::397 => if (reason.length == 0) {
contracts/abstract/JB721Delegate.sol::120 => _data.metadata.length < 4 || bytes4(_data.metadata[0:4]) != type(IJB721Delegate).interfaceId
contracts/abstract/JB721Delegate.sol::259 => _data.metadata.length < 4 || bytes4(_data.metadata[0:4]) != type(IJB721Delegate).interfaceId
contracts/abstract/JB721Delegate.sol::266 => uint256 _numberOfTokenIds = _decodedTokenIds.length;
contracts/forge-test/E2E.t.sol::323 => uint256[] memory _tiersToRemove = new uint256[](_originalTiers.length);
contracts/forge-test/E2E.t.sol::326 => for (uint256 _i; _i < _originalTiers.length; _i++) {
contracts/forge-test/E2E.t.sol::603 => for (uint256 i; i < rawMetadata.length; i++) rawMetadata[i] = uint16(tier);
contracts/forge-test/E2E.t.sol::615 => _jbETHPaymentTerminal.pay{value: floor * rawMetadata.length}(
contracts/forge-test/E2E.t.sol::637 => assertEq(rawMetadata.length, tokenBalance);
contracts/forge-test/E2E.t.sol::642 => for (uint256 i; i < rawMetadata.length; i++) {
contracts/forge-test/E2E.t.sol::684 => _jbETHPaymentTerminal.pay{value: floor * rawMetadata.length}(
contracts/forge-test/E2E.t.sol::704 => assertEq(rawMetadata.length, tokenBalance);
contracts/forge-test/NFTReward_Unit.t.sol::345 => _delegate.test_store().tiers(address(_delegate), 0, numberOfTiers).length,
contracts/forge-test/NFTReward_Unit.t.sol::803 => for (uint256 i; i < _tiers.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::839 => for (uint256 i = 1; i <= _tiers.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::1716 => _tiersToMint[_tiersToMint.length - 1 - i] = uint16(i)+1;
contracts/forge-test/NFTReward_Unit.t.sol::1785 => _tiersToMintOne[_tiersToMintOne.length - 1 - i] = uint16(i)+1;
contracts/forge-test/NFTReward_Unit.t.sol::1788 => _tiersToMintTwo[_tiersToMintTwo.length - 1 - i] = uint16(i)+1;
contracts/forge-test/NFTReward_Unit.t.sol::1864 => _tiersToMint[_tiersToMint.length - 1 - i] = uint16(i)+1;
contracts/forge-test/NFTReward_Unit.t.sol::1952 => vm.assume(floorTiersToAdd.length > 0 && floorTiersToAdd.length < 15);
contracts/forge-test/NFTReward_Unit.t.sol::2015 => JB721TierParams[] memory _tierParamsToAdd = new JB721TierParams[](floorTiersToAdd.length);
contracts/forge-test/NFTReward_Unit.t.sol::2016 => JB721Tier[] memory _tiersAdded = new JB721Tier[](floorTiersToAdd.length);
contracts/forge-test/NFTReward_Unit.t.sol::2018 => for (uint256 i; i < floorTiersToAdd.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::2031 => id: _tiers.length + (i + 1),
contracts/forge-test/NFTReward_Unit.t.sol::2052 => initialNumberOfTiers + floorTiersToAdd.length
contracts/forge-test/NFTReward_Unit.t.sol::2056 => assertEq(_storedTiers.length, _tiers.length + _tiersAdded.length);
contracts/forge-test/NFTReward_Unit.t.sol::2063 => for (uint256 i = 1; i < _storedTiers.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::2173 => for (uint256 i; i < _tiers.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::2178 => for (uint256 i; i < tiersRemaining.length; ) {
contracts/forge-test/NFTReward_Unit.t.sol::2181 => for (uint256 j; j < tiersToRemove.length; j++)
contracts/forge-test/NFTReward_Unit.t.sol::2184 => tiersRemaining[i] = tiersRemaining[tiersRemaining.length - 1];
contracts/forge-test/NFTReward_Unit.t.sol::2185 => tierParamsRemaining[i] = tierParamsRemaining[tierParamsRemaining.length - 1];
contracts/forge-test/NFTReward_Unit.t.sol::2187 => // Remove the last elelment / reduce array length by 1
contracts/forge-test/NFTReward_Unit.t.sol::2201 => for (uint256 i; i < tiersToRemove.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::2210 => uint256 finalNumberOfTiers = initialNumberOfTiers - tiersToRemove.length;
contracts/forge-test/NFTReward_Unit.t.sol::2219 => assertEq(_storedTiers.length, finalNumberOfTiers);
contracts/forge-test/NFTReward_Unit.t.sol::2373 => id: _tiers.length + (i + 1),
contracts/forge-test/NFTReward_Unit.t.sol::2398 => assertEq(_storedTiers.length, 7);
contracts/forge-test/NFTReward_Unit.t.sol::2408 => for (uint256 j = 1; j < _storedTiers.length; j++) {
contracts/forge-test/NFTReward_Unit.t.sol::2490 => id: _tiers.length + (i + 1),
contracts/forge-test/NFTReward_Unit.t.sol::2593 => id: _tiers.length + (i + 1),
contracts/forge-test/NFTReward_Unit.t.sol::2696 => id: _tiers.length + (i + 1),
contracts/forge-test/NFTReward_Unit.t.sol::2799 => _delegate.store().tiers(address(_delegate), 0, initialNumberOfTiers).length,
contracts/forge-test/NFTReward_Unit.t.sol::2904 => for (uint256 i; i < _tiers.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::2909 => for (uint256 i; i < tiersRemaining.length; ) {
contracts/forge-test/NFTReward_Unit.t.sol::2912 => for (uint256 j; j < tiersToRemove.length; j++)
contracts/forge-test/NFTReward_Unit.t.sol::2915 => tiersRemaining[i] = tiersRemaining[tiersRemaining.length - 1];
contracts/forge-test/NFTReward_Unit.t.sol::2916 => tierParamsRemaining[i] = tierParamsRemaining[tierParamsRemaining.length - 1];
contracts/forge-test/NFTReward_Unit.t.sol::2918 => // Remove the last elelment / reduce array length by 1
contracts/forge-test/NFTReward_Unit.t.sol::3580 => vm.assume(_invalidTier > tiers.length);
contracts/forge-test/NFTReward_Unit.t.sol::4873 => assertEq(first.length, second.length);
contracts/forge-test/NFTReward_Unit.t.sol::4875 => for (uint256 i; i < first.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::4904 => if (smol.length > bigg.length) {
contracts/forge-test/NFTReward_Unit.t.sol::4909 => if (smol.length == 0) return true;
contracts/forge-test/NFTReward_Unit.t.sol::4914 => for (uint256 smolIter; smolIter < smol.length; smolIter++) {
contracts/forge-test/NFTReward_Unit.t.sol::4916 => for (uint256 biggIter; biggIter < bigg.length; biggIter++) {
contracts/forge-test/NFTReward_Unit.t.sol::4919 => count += smolIter + 1; // 1-indexed, as the length
contracts/forge-test/NFTReward_Unit.t.sol::4925 => // Insure all the smoll indexes have been iterated on (ie we've seen (smoll.length)! elements)
contracts/forge-test/NFTReward_Unit.t.sol::4926 => if (count == (smol.length * (smol.length + 1)) / 2) return true;
contracts/forge-test/NFTReward_Unit.t.sol::4950 => for (uint256 i; i < _in.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::4954 => for (uint256 j = i; j < _in.length; j++) {
contracts/forge-test/NFTReward_Unit.t.sol::4968 => for (uint256 i; i < _in.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::4972 => for (uint256 j = i; j < _in.length; j++) {
contracts/forge-test/NFTReward_Unit.t.sol::4986 => for (uint256 i; i < _in.length; i++) {
contracts/forge-test/NFTReward_Unit.t.sol::4990 => for (uint256 j = i; j < _in.length; j++) {
contracts/forge-test/NFTReward_Unit.t.sol::5095 => // Initialize an array with the appropriate length.
contracts/forge-test/NFTReward_Unit.t.sol::5129 => for (uint256 i = _tiers.length - 1; i >= 0; i--) {
contracts/forge-test/governance/JB721GlobalGovernance.t.sol::33 => vm.assume(_tier < NFTRewardDeployerData.pricing.tiers.length);
contracts/forge-test/governance/JB721GlobalGovernance.t.sol::102 => vm.assume(_tier < NFTRewardDeployerData.pricing.tiers.length);
contracts/forge-test/governance/JB721TieredGovernance.t.sol::33 => vm.assume(_tier < NFTRewardDeployerData.pricing.tiers.length);
contracts/forge-test/governance/JB721TieredGovernance.t.sol::107 => vm.assume(_tier < NFTRewardDeployerData.pricing.tiers.length);
contracts/libraries/JBIpfsDecoder.sol::45 => if (_source.length == 0) return new string(0);
contracts/libraries/JBIpfsDecoder.sol::48 => uint8 digitlength = 1;
contracts/libraries/JBIpfsDecoder.sol::49 => for (uint256 i = 0; i < _source.length; ++i) {
contracts/libraries/JBIpfsDecoder.sol::51 => for (uint256 j = 0; j < digitlength; ++j) {
contracts/libraries/JBIpfsDecoder.sol::58 => digits[digitlength] = uint8(carry % 58);
contracts/libraries/JBIpfsDecoder.sol::59 => digitlength++;
contracts/libraries/JBIpfsDecoder.sol::63 => return string(_toAlphabet(_reverse(_truncate(digits, digitlength))));
contracts/libraries/JBIpfsDecoder.sol::66 => function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
contracts/libraries/JBIpfsDecoder.sol::67 => uint8[] memory output = new uint8[](_length);
contracts/libraries/JBIpfsDecoder.sol::68 => for (uint256 i = 0; i < _length; i++) {
contracts/libraries/JBIpfsDecoder.sol::75 => uint8[] memory output = new uint8[](_input.length);
contracts/libraries/JBIpfsDecoder.sol::76 => for (uint256 i = 0; i < _input.length; i++) {
contracts/libraries/JBIpfsDecoder.sol::77 => output[i] = _input[_input.length - 1 - i];
contracts/libraries/JBIpfsDecoder.sol::83 => bytes memory output = new bytes(_indices.length);
contracts/libraries/JBIpfsDecoder.sol::84 => for (uint256 i = 0; i < _indices.length; i++) {
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Use != 0 instead of > 0 for Unsigned Integer Comparison

#### Impact
Issue Information: [G003](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g003---use--0-instead-of--0-for-unsigned-integer-comparison)

#### Findings:
```
contracts/JBTiered721Delegate.sol::240 => if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);
contracts/JBTiered721DelegateStore.sol::1254 => if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
contracts/abstract/ERC721.sol::112 => return bytes(baseURI).length > 0 ? string(abi.encodePacked(baseURI, tokenId.toString())) : '';
contracts/abstract/JB721Delegate.sol::116 => if (_data.tokenCount > 0) revert UNEXPECTED_TOKEN_REDEEMED();
contracts/abstract/Votes.sol::154 => if (from != to && amount > 0) {
contracts/forge-test/NFTReward_Unit.t.sol::209 => vm.assume(numberOfTiers > 0 && numberOfTiers < 30);
contracts/forge-test/NFTReward_Unit.t.sol::276 => vm.assume(numberOfTiers > 0 && numberOfTiers < 30);
contracts/forge-test/NFTReward_Unit.t.sol::368 => vm.assume(numberOfTiers > 0 && numberOfTiers < 30);
contracts/forge-test/NFTReward_Unit.t.sol::469 => vm.assume(numberOfTiers > 0 && numberOfTiers < 30);
contracts/forge-test/NFTReward_Unit.t.sol::539 => vm.assume(numberOfTiers > 0 && numberOfTiers < 30);
contracts/forge-test/NFTReward_Unit.t.sol::671 => vm.assume(numberOfTiers > 0 && numberOfTiers < 30);
contracts/forge-test/NFTReward_Unit.t.sol::729 => vm.assume(_tierId > 0 && _tokenNumber > 0);
contracts/forge-test/NFTReward_Unit.t.sol::862 => vm.assume(numberOfTiers > 0 && numberOfTiers < 30);
contracts/forge-test/NFTReward_Unit.t.sol::932 => vm.assume(numberOfTiers > 0 && numberOfTiers < 30);
contracts/forge-test/NFTReward_Unit.t.sol::1952 => vm.assume(floorTiersToAdd.length > 0 && floorTiersToAdd.length < 15);
contracts/forge-test/NFTReward_Unit.t.sol::2083 => vm.assume(initialNumberOfTiers > 0 && initialNumberOfTiers < 15);
contracts/forge-test/NFTReward_Unit.t.sol::2084 => vm.assume(numberOfTiersToRemove > 0 && numberOfTiersToRemove < initialNumberOfTiers);
contracts/forge-test/NFTReward_Unit.t.sol::2419 => vm.assume(numberTiersToAdd > 0);
contracts/forge-test/NFTReward_Unit.t.sol::2522 => vm.assume(numberTiersToAdd > 0);
contracts/forge-test/NFTReward_Unit.t.sol::2625 => vm.assume(numberTiersToAdd > 0);
contracts/forge-test/NFTReward_Unit.t.sol::2722 => vm.assume(initialNumberOfTiers > 0 && initialNumberOfTiers < 15);
contracts/forge-test/NFTReward_Unit.t.sol::2814 => vm.assume(initialNumberOfTiers > 0 && initialNumberOfTiers < 15);
contracts/forge-test/NFTReward_Unit.t.sol::2815 => vm.assume(numberOfTiersToRemove > 0 && numberOfTiersToRemove < initialNumberOfTiers);
contracts/forge-test/NFTReward_Unit.t.sol::3260 => vm.assume(_amount > 0 && _amount < tiers[0].contributionFloor);
contracts/forge-test/NFTReward_Unit.t.sol::4561 => vm.assume(_tokenCount > 0);
contracts/libraries/JBIpfsDecoder.sol::57 => while (carry > 0) {
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Use immutable for OpenZeppelin AccessControl's Roles Declarations

#### Impact
Issue Information: [G006](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md#g006---use-immutable-for-openzeppelin-accesscontrols-roles-declarations)

#### Findings:
```
contracts/forge-test/Deployer_Unit.t.sol::16 => address owner = address(bytes20(keccak256('owner')));
contracts/forge-test/Deployer_Unit.t.sol::17 => address reserveBeneficiary = address(bytes20(keccak256('reserveBeneficiary')));
contracts/forge-test/Deployer_Unit.t.sol::18 => address mockJBDirectory = address(bytes20(keccak256('mockJBDirectory')));
contracts/forge-test/Deployer_Unit.t.sol::19 => address mockTokenUriResolver = address(bytes20(keccak256('mockTokenUriResolver')));
contracts/forge-test/Deployer_Unit.t.sol::20 => address mockTerminalAddress = address(bytes20(keccak256('mockTerminalAddress')));
contracts/forge-test/Deployer_Unit.t.sol::21 => address mockJBController = address(bytes20(keccak256('mockJBController')));
contracts/forge-test/Deployer_Unit.t.sol::22 => address mockJBFundingCycleStore = address(bytes20(keccak256('mockJBFundingCycleStore')));
contracts/forge-test/Deployer_Unit.t.sol::23 => address mockJBOperatorStore = address(bytes20(keccak256('mockJBOperatorStore')));
contracts/forge-test/Deployer_Unit.t.sol::24 => address mockJBProjects = address(bytes20(keccak256('mockJBProjects')));
contracts/forge-test/E2E.t.sol::16 => address reserveBeneficiary = address(bytes20(keccak256('reserveBeneficiary')));
contracts/forge-test/E2E.t.sol::287 => address _user = address(bytes20(keccak256('user')));
contracts/forge-test/NFTReward_Unit.t.sol::17 => address beneficiary = address(bytes20(keccak256('beneficiary')));
contracts/forge-test/NFTReward_Unit.t.sol::18 => address owner = address(bytes20(keccak256('owner')));
contracts/forge-test/NFTReward_Unit.t.sol::19 => address reserveBeneficiary = address(bytes20(keccak256('reserveBeneficiary')));
contracts/forge-test/NFTReward_Unit.t.sol::20 => address mockJBDirectory = address(bytes20(keccak256('mockJBDirectory')));
contracts/forge-test/NFTReward_Unit.t.sol::21 => address mockJBFundingCycleStore = address(bytes20(keccak256('mockJBFundingCycleStore')));
contracts/forge-test/NFTReward_Unit.t.sol::22 => address mockTokenUriResolver = address(bytes20(keccak256('mockTokenUriResolver')));
contracts/forge-test/NFTReward_Unit.t.sol::23 => address mockTerminalAddress = address(bytes20(keccak256('mockTerminalAddress')));
contracts/forge-test/NFTReward_Unit.t.sol::24 => address mockJBProjects = address(bytes20(keccak256('mockJBProjects')));
contracts/forge-test/NFTReward_Unit.t.sol::67 => address delegate_i = address(bytes20(keccak256('delegate_implementation')));
contracts/forge-test/NFTReward_Unit.t.sol::1080 => address(bytes20(keccak256('previousOne')))
contracts/forge-test/NFTReward_Unit.t.sol::1756 => address beneficiaryTwo = address(bytes20(keccak256('beneficiaryTwo')));
contracts/forge-test/NFTReward_Unit.t.sol::1920 => address(bytes20(keccak256('newReserveBeneficiary')))
contracts/forge-test/NFTReward_Unit.t.sol::1942 => address(bytes20(keccak256('newOne')))
contracts/forge-test/NFTReward_Unit.t.sol::2091 => uint256 _newTierCandidate = uint256(keccak256(abi.encode(seed))) % initialNumberOfTiers;
contracts/forge-test/NFTReward_Unit.t.sol::2822 => uint256 _newTierCandidate = uint256(keccak256(abi.encode(seed))) % initialNumberOfTiers;
contracts/forge-test/NFTReward_Unit.t.sol::3433 => address _jbPrice = address(bytes20(keccak256('MockJBPrice')));
contracts/forge-test/NFTReward_Unit.t.sol::4097 => address _holder = address(bytes20(keccak256('_holder')));
contracts/forge-test/NFTReward_Unit.t.sol::4593 => address _holder = address(bytes20(keccak256('_holder')));
contracts/forge-test/NFTReward_Unit.t.sol::4696 => address _holder = address(bytes20(keccak256('_holder')));
contracts/forge-test/NFTReward_Unit.t.sol::4742 => address _holder = address(bytes20(keccak256('_holder')));
contracts/forge-test/NFTReward_Unit.t.sol::4785 => address _holder = address(bytes20(keccak256('_holder')));
contracts/forge-test/governance/JB721GlobalGovernance.t.sol::11 => address _user = address(bytes20(keccak256('user')));
contracts/forge-test/governance/JB721GlobalGovernance.t.sol::12 => address _userFren = address(bytes20(keccak256('user_fren')));
contracts/forge-test/governance/JB721GlobalGovernance.t.sol::80 => address _user = address(bytes20(keccak256('user')));
contracts/forge-test/governance/JB721GlobalGovernance.t.sol::81 => address _userFren = address(bytes20(keccak256('user_fren')));
contracts/forge-test/governance/JB721TieredGovernance.t.sol::11 => address _user = address(bytes20(keccak256('user')));
contracts/forge-test/governance/JB721TieredGovernance.t.sol::12 => address _userFren = address(bytes20(keccak256('user_fren')));
contracts/forge-test/governance/JB721TieredGovernance.t.sol::85 => address _user = address(bytes20(keccak256('user')));
contracts/forge-test/governance/JB721TieredGovernance.t.sol::86 => address _userFren = address(bytes20(keccak256('user_fren')));
contracts/forge-test/utils/TestBaseWorkflow.sol::208 => bytes32 hash = keccak256(data);
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Use Shift Right/Left instead of Division/Multiplication if possible

#### Impact
Issue Information: [G008](https://github.com/byterocket/c4-common-issues/blob/main/0-Gas-Optimizations.md/#g008---use-shift-rightleft-instead-of-divisionmultiplication-if-possible)

#### Findings:
```
contracts/forge-test/NFTReward_Unit.t.sol::527 => ((numberOfTiers * (numberOfTiers + 1)) / 2)
contracts/forge-test/NFTReward_Unit.t.sol::583 => assertEq(_delegate.balanceOf(holder), 10 * ((numberOfTiers * (numberOfTiers + 1)) / 2));
contracts/forge-test/NFTReward_Unit.t.sol::717 => 10 * ((numberOfTiers * (numberOfTiers - 1)) / 2) // (numberOfTiers-1)! * 10
contracts/forge-test/NFTReward_Unit.t.sol::868 => uint256 _maxNumberOfTiers = (numberOfTiers * (numberOfTiers + 1)) / 2; // "tier amount" of token mintable per tier -> max == numberOfTiers!
contracts/forge-test/NFTReward_Unit.t.sol::1699 => uint16[] memory _tiersToMint = new uint16[](nbTiers * 2);
contracts/forge-test/NFTReward_Unit.t.sol::1766 => uint16[] memory _tiersToMintOne = new uint16[](nbTiers * 2);
contracts/forge-test/NFTReward_Unit.t.sol::1767 => uint16[] memory _tiersToMintTwo = new uint16[](nbTiers * 2);
contracts/forge-test/NFTReward_Unit.t.sol::1847 => uint16[] memory _tiersToMint = new uint16[](nbTiers * 2);
contracts/forge-test/NFTReward_Unit.t.sol::3087 => tiers[0].contributionFloor * 2 + tiers[1].contributionFloor,
contracts/forge-test/NFTReward_Unit.t.sol::3349 => uint256 _amount = tiers[0].contributionFloor * 2 + tiers[1].contributionFloor + _leftover / 2;
contracts/forge-test/NFTReward_Unit.t.sol::3468 => uint256 _amountInOtherCurrency = tiers[0].contributionFloor * 2 + tiers[1].contributionFloor;
contracts/forge-test/NFTReward_Unit.t.sol::3469 => uint256 _amountInEth = (tiers[0].contributionFloor * 2 + tiers[1].contributionFloor) * 2;
contracts/forge-test/NFTReward_Unit.t.sol::3560 => tiers[0].contributionFloor * 2 + tiers[1].contributionFloor,
contracts/forge-test/NFTReward_Unit.t.sol::3621 => tiers[0].contributionFloor * 2 + tiers[1].contributionFloor,
contracts/forge-test/NFTReward_Unit.t.sol::3679 => tiers[0].contributionFloor * 2 + tiers[1].contributionFloor - 1,
contracts/forge-test/NFTReward_Unit.t.sol::3955 => tiers[0].contributionFloor * 2 + tiers[1].contributionFloor,
contracts/forge-test/NFTReward_Unit.t.sol::4073 => tiers[0].contributionFloor * 2 + tiers[1].contributionFloor,
contracts/forge-test/NFTReward_Unit.t.sol::4227 => uint256 _redemptionRate = 4000; // 40%
contracts/forge-test/NFTReward_Unit.t.sol::4451 => uint256 _redemptionRate = _accessJBLib.MAX_RESERVED_RATE(); // 40%
contracts/forge-test/NFTReward_Unit.t.sol::4926 => if (count == (smol.length * (smol.length + 1)) / 2) return true;
contracts/forge-test/utils/TestBaseWorkflow.sol::195 => //https://ethereum.stackexchange.com/questions/24248/how-to-calculate-an-ethereum-contracts-address-during-its-creation-using-the-so
contracts/libraries/JBBitmap.sol::74 => return _index / 256;
contracts/libraries/JBIpfsDecoder.sol::52 => carry += uint256(digits[j]) * 256;
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)