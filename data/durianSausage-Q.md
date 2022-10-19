### L01:_SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE
#### problem
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### prof
JBTiered721Delegate.sol, 461, b'      _mint(_reservedTokenBeneficiary, _tokenId);'
JBTiered721Delegate.sol, 504, b'      _mint(_beneficiary, _tokenId);'
JBTiered721Delegate.sol, 635, b'    _mint(_beneficiary, _tokenId);'
JBTiered721Delegate.sol, 677, b'      _mint(_beneficiary, _tokenId);'

### L02: INPUT ARRAY LENGTHS MAY DIFFER
#### problem
If the caller makes a copy-paste error, the lengths may be mismatchd and an operation believed to have been completed may not in fact have been completed
function withdrawMultipleERC721(address[] memory _tokens, uint256[] memory _tokenId, address _to) external override {
#### prof
JBTiered721Delegate.sol, 359, b'  function adjustTiers(JB721TierParams[] calldata _tiersToAdd, uint256[] calldata _tierIdsToRemove)\n    external\n    override\n    onlyOwner\n  '


### L03: address variable should check if it is zero MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
#### prof
JBTiered721Delegate.sol, 223, b'    fundingCycleStore = _fundingCycleStore;'
JBTiered721Delegate.sol, 224, b'    store = _store;'
JBTiered721Delegate.sol, 227, b'    prices = _pricing.prices;'
JBTiered721DelegateDeployer.sol, 51, b'    globalGovernance = _globalGovernance;'
JBTiered721DelegateDeployer.sol, 52, b'    tieredGovernance = _tieredGovernance;'
JBTiered721DelegateDeployer.sol, 53, b'    noGovernance = _noGovernance;'
abstract/JB721Delegate.sol, 212, b'    directory = _directory;'
JBTiered721DelegateProjectDeployer.sol, 52, b'    controller = _controller;'
JBTiered721DelegateProjectDeployer.sol, 53, b'    delegateDeployer = _delegateDeployer;'


