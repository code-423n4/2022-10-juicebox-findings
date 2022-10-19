

### G01: COMPARISONS WITH ZERO FOR UNSIGNED INTEGERS
#### problem
0 is less gas efficient than !0 if you enable the optimizer at 10k AND you’re in a require statement. Detailed explanation with the opcodes https://twitter.com/gzeon/status/1485428085885640706
#### prof
libraries/JBIpfsDecoder.sol, 57, b'      while (carry > 0) {'
JBTiered721Delegate.sol, 240, b'    if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);'
JBTiered721DelegateStore.sol, 1254, b'    if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)'
abstract/JB721Delegate.sol, 116, b'    if (_data.tokenCount > 0) revert UNEXPECTED_TOKEN_REDEEMED();'


### G02: PREFIX INCREMENT SAVE MORE GAS
#### problem
prefix increment ++i is more cheaper than postfix i++
#### prof
libraries/JBIpfsDecoder.sol, 59, b'        digitlength++;'
libraries/JBIpfsDecoder.sol, 68, b'    for (uint256 i = 0; i < _length; i++) {'
libraries/JBIpfsDecoder.sol, 76, b'    for (uint256 i = 0; i < _input.length; i++) {'
libraries/JBIpfsDecoder.sol, 84, b'    for (uint256 i = 0; i < _indices.length; i++) {'
JBTiered721DelegateStore.sol, 1106, b'      numberOfBurnedFor[msg.sender][_tierId]++;'
JBTiered721DelegateStore.sol, 1108, b'      _storedTierOf[msg.sender][_tierId].remainingQuantity++;'

### G03: X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES
#### prof
JBTiered721DelegateStore.sol, 827, b'    numberOfReservesMintedFor[msg.sender][_tierId] += _count;'

### G04: resign the default value to the variables.
#### problem
 resign the default value to the variables will cost more gas.
#### prof
libraries/JBIpfsDecoder.sol, 51, b'      for (uint256 j = 0; j < digitlength; ++j) {'
libraries/JBIpfsDecoder.sol, 49, b'    for (uint256 i = 0; i < _source.length; ++i) {'
libraries/JBIpfsDecoder.sol, 68, b'    for (uint256 i = 0; i < _length; i++) {'
libraries/JBIpfsDecoder.sol, 76, b'    for (uint256 i = 0; i < _input.length; i++) {'
libraries/JBIpfsDecoder.sol, 84, b'    for (uint256 i = 0; i < _indices.length; i++) {'

### G05: ++I/I++ SHOULD BE UNCHECKED{++I}/UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS
#### problem
The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop
#### prof
libraries/JBIpfsDecoder.sol, 51, b'      for (uint256 j = 0; j < digitlength; ++j) {'
libraries/JBIpfsDecoder.sol, 59, b'        digitlength++;'
libraries/JBIpfsDecoder.sol, 49, b'    for (uint256 i = 0; i < _source.length; ++i) {'
libraries/JBIpfsDecoder.sol, 68, b'    for (uint256 i = 0; i < _length; i++) {'
libraries/JBIpfsDecoder.sol, 76, b'    for (uint256 i = 0; i < _input.length; i++) {'
libraries/JBIpfsDecoder.sol, 84, b'    for (uint256 i = 0; i < _indices.length; i++) {'

### G06: FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE
#### problem
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost
#### prof
JBTiered721Delegate.sol, 109, b'  function firstOwnerOf(uint256 _tokenId) external view override returns (address) '
JBTiered721Delegate.sol, 109, b'  function firstOwnerOf(uint256 _tokenId) external view override returns (address) '
JBTiered721Delegate.sol, 125, b'  function balanceOf(address _owner) public view override returns (uint256 balance) '
JBTiered721Delegate.sol, 125, b'  function balanceOf(address _owner) public view override returns (uint256 balance) '
JBTiered721Delegate.sol, 154, b'  function tokenURI(uint256 _tokenId) public view override returns (string memory) '
JBTiered721Delegate.sol, 252, b'  function initialize(\n    uint256 _projectId,\n    IJBDirectory _directory,\n    string memory _name,\n    string memory _symbol,\n    IJBFundingCycleStore _fundingCycleStore,\n    string memory _baseUri,\n    IJBTokenUriResolver _tokenUriResolver,\n    string memory _contractUri,\n    JB721PricingParams memory _pricing,\n    IJBTiered721DelegateStore _store,\n    JBTiered721Flags memory _flags\n  ) public override '
JBTiered721Delegate.sol, 309, b'  function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)\n    external\n    override\n    onlyOwner\n  '
JBTiered721Delegate.sol, 359, b'  function adjustTiers(JB721TierParams[] calldata _tiersToAdd, uint256[] calldata _tierIdsToRemove)\n    external\n    override\n    onlyOwner\n  '
JBTiered721Delegate.sol, 375, b'  function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner '
JBTiered721Delegate.sol, 375, b'  function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner '
JBTiered721Delegate.sol, 391, b'  function setBaseUri(string memory _baseUri) external override onlyOwner '
JBTiered721Delegate.sol, 391, b'  function setBaseUri(string memory _baseUri) external override onlyOwner '
JBTiered721Delegate.sol, 407, b'  function setContractUri(string calldata _contractUri) external override onlyOwner '
JBTiered721Delegate.sol, 407, b'  function setContractUri(string calldata _contractUri) external override onlyOwner '
JBTiered721Delegate.sol, 423, b'  function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner '
JBTiered721Delegate.sol, 423, b'  function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner '
JBTiered721Delegate.sol, 512, b'  function mintFor(uint16[] memory _tierIds, address _beneficiary)\n    public\n    override\n    onlyOwner\n    returns (uint256[] memory tokenIds)\n  '
JBTiered721Delegate.sol, 749, b'  function _beforeTokenTransfer(\n    address _from,\n    address _to,\n    uint256 _tokenId\n  ) internal virtual override '
JBTiered721DelegateDeployer.sol, 105, b'  function deployDelegateFor(\n    uint256 _projectId,\n    JBDeployTiered721DelegateData memory _deployTiered721DelegateData\n  ) external override returns (IJBTiered721Delegate) '
JBTiered721DelegateStore.sol, 512, b'  function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) '
JBTiered721DelegateStore.sol, 512, b'  function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) '
JBTiered721DelegateStore.sol, 1125, b'  function recordSetFirstOwnerOf(uint256 _tokenId, address _owner) external override '
JBTiered721DelegateStore.sol, 1125, b'  function recordSetFirstOwnerOf(uint256 _tokenId, address _owner) external override '
abstract/JB721Delegate.sol, 289, b'  function didRedeem(JBDidRedeemData calldata _data) external payable virtual override '
JBTiered721DelegateProjectDeployer.sol, 92, b'  function launchProjectFor(\n    address _owner,\n    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,\n    JBLaunchProjectData memory _launchProjectData\n  ) external override returns (uint256 projectId) '
JBTiered721DelegateProjectDeployer.sol, 92, b'  function launchProjectFor(\n    address _owner,\n    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,\n    JBLaunchProjectData memory _launchProjectData\n  ) external override returns (uint256 projectId) '
JBTiered721DelegateProjectDeployer.sol, 135, b'  function launchFundingCyclesFor(\n    uint256 _projectId,\n    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,\n    JBLaunchFundingCyclesData memory _launchFundingCyclesData\n  )\n    external\n    override\n    requirePermission(\n      controller.projects().ownerOf(_projectId),\n      _projectId,\n      JBOperations.RECONFIGURE\n    )\n    returns (uint256 configuration)\n  '
JBTiered721DelegateProjectDeployer.sol, 178, b'  function reconfigureFundingCyclesOf(\n    uint256 _projectId,\n    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,\n    JBReconfigureFundingCyclesData memory _reconfigureFundingCyclesData\n  )\n    external\n    override\n    requirePermission(\n      controller.projects().ownerOf(_projectId),\n      _projectId,\n      JBOperations.RECONFIGURE\n    )\n    returns (uint256 configuration)\n  '
JBTiered721DelegateProjectDeployer.sol, 205, b'  function _launchProjectFor(address _owner, JBLaunchProjectData memory _launchProjectData)\n    internal\n  '
JBTiered721DelegateProjectDeployer.sol, 205, b'  function _launchProjectFor(address _owner, JBLaunchProjectData memory _launchProjectData)\n    internal\n  '

### G07: USING PRIVATE RATHER THAN PUBLIC FOR CONSTANTS, SAVES GAS
#### problem:
We can save getter function of public constants.
#### prof:
JBTiered721DelegateDeployer.sol, 28, b'  JB721GlobalGovernance public immutable globalGovernance;'
JBTiered721DelegateDeployer.sol, 34, b'  JB721TieredGovernance public immutable tieredGovernance;'
JBTiered721DelegateDeployer.sol, 40, b'  JBTiered721Delegate public immutable noGovernance;'
JBTiered721DelegateProjectDeployer.sol, 30, b'  IJBController public immutable override controller;'
JBTiered721DelegateProjectDeployer.sol, 36, b'  IJBTiered721DelegateDeployer public immutable override delegateDeployer;'