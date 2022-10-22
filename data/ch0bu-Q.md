# NON-CRITICAL

## 1.  Non-library/interface files should use fixed compiler versions, not floating ones


In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Proof of Concept
https://swcregistry.io/docs/SWC-103

```
All contracts have floating pragmas
```

## 2. NatSpec is incomplete


Check here for NatSpec rules:
https://docs.soliditylang.org/en/develop/natspec-format.html

```
missing @author
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol
```
missing @author 
missing @title
...
missing @param _targetAddress, @return _out
115		function _clone(address _targetAddress) internal returns (address _out) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol
```
missing @author 
missing @title
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol
```
missing @author 
missing @return
74		function getTierDelegate(address _account, uint256 _tier)
missing @return
90		function getTierVotes(address _account, uint256 _tier) external view override returns (uint256) {
missing @return
102		function getPastTierVotes(
missing @return
116		function getTierTotalVotes(uint256 _tier) external view override returns (uint256) {
missing @return
127		function getPastTierTotalVotes(uint256 _tier, uint256 _blockNumber)
missing @return
194		function _getTierVotingUnits(address _account, uint256 _tierId)
missing @param a, @param b, @ return
322		function _add(uint256 a, uint256 b) internal pure returns (uint256) {
missing @param a, @param b, @ return
326		function _subtract(uint256 a, uint256 b) internal pure returns (uint256) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol
```
missing @author 
missing @return
175		function supportsInterface(bytes4 _interfaceId) public view override returns (bool) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol
```
missing @author 
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol
```
missing @author
missing @return
178		function supportsInterface(bytes4 _interfaceId)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol
```
missing @author 
missing @title
missing @param _baseUri, @param _hexString, @return 
22		function decode(string memory _baseUri, bytes32 _hexString)
missing @param _source, @return
44		function _toBase58(bytes memory _source) private pure returns (string memory) {
missing @param _array, @param _length, @ return
66		function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
missing @param _input, @return
74		function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {
missing @param _indices, @ return
82		function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol


## 3. File is missing NatSpec


It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI).

```
Missing NatSpec
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol

## 4. `public` functions not called by the contract should be declared `external` instead


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


## 5. Not using the named return variables anywhere in the function is confusing


Consider changing the variable to be an unnamed one

```
37		returns (uint256 units)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol
```
72		) external override returns (IJBTiered721Delegate) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol
```
162		returns (uint256 configuration)
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol
```
123		function balanceOf(address _owner) public view override returns (uint256 balance) {
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol


# LOW

## 1. Setters should check the input value


Setters should check the input value - ie make revert if it is the zero address or zero

```
370		function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {
371			// Set the beneficiary.
372			store.recordSetDefaultReservedTokenBeneficiary(_beneficiary);
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol
```
177		function setTierDelegate(address _delegatee, uint256 _tierId) public virtual override {
178			_delegateTier(msg.sender, _delegatee, _tierId);
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol
```
854		function recordSetDefaultReservedTokenBeneficiary(address _beneficiary) external override {
855			defaultReservedTokenBeneficiaryOf[msg.sender] = _beneficiary;
...
1123		function recordSetFirstOwnerOf(uint256 _tokenId, address _owner) external override {
1124			firstOwnerOf[msg.sender][_tokenId] = _owner;
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

## 2. Add constructor initializers

As per OpenZeppelin’s (OZ) [recommendation](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/6), “The guidelines are now to make it impossible for anyone to run initialize on an implementation contract, by adding an empty constructor with the initializer modifier. So the implementation contract gets initialized automatically upon deployment.”

Note that this behaviour is also incorporated the OZ Wizard since the UUPS vulnerability discovery: “Additionally, we modified the code generated by the Wizard 19 to include a constructor that automatically initializes the implementation when deployed.”

```
202		function initialize(
```
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol



