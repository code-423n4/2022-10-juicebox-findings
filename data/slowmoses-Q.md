## Avoid Magic Numbers

Checking against magic numbers should be avoided.
Consider using a properly named constant instead.

***

_data.metadata.length > 36 &&
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L551

tokenId |= _tokenNumber << 16;
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1279

## Revert and Require Should Have Descriptive Reason Strings

require(address(this) != codeOrigin);
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L216

require(address(store) == address(0));
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L218


## Avoid Overlong Lines

Limit line length to 164 characters.
Longer lines will need scrollbars in github and are hard to read anyways.

***

  IJBTiered721DelegateDeployer: General interface for the generic controller methods in this contract that interacts with funding cycles and tokens according to the protocol's rules.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L15

  IJBTiered721DelegateProjectDeployer: General interface for the generic controller methods in this contract that interacts with funding cycles and tokens according to the protocol's rules.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L15

  JBOperatable: Several functions in this contract can only be accessed by a project owner, or an address that has been preconfifigured to be an operator of the project.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L19

    Transfers, mints, or burns tier voting units. To register a mint, `from` should be zero. To register a burn, `to` should be zero. Total supply of voting units will be adjusted with mints and burns.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L235

  Delegate that offers project contributors NFTs with tiered price floors upon payment and the ability to redeem NFTs for treasury assets based based on price floor.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L17

    @param _pricing The tier pricing according to which token distribution will be made. Must be passed in order of contribution floor, with implied increasing value.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L198

    // Keep a reference to the flag indicating if the transaction should revert if all provded funds aren't spent. Defaults to false, meaning only a minimum payment is enforced.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L545

    Part of IJBFundingCycleDataSource, this function gets called when the project receives a payment. It will set itself as the delegate to get a callback from the terminal.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L70

    Part of IJBPayDelegate, this function gets called when the project receives a payment. It will mint an NFT to the contributor (_data.beneficiary) if conditions are met.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L221

    Part of IJBRedeemDelegate, this function gets called when the token holder redeems. It will burn the specified NFTs to reclaim from the treasury to the _data.beneficiary.

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L242

## Missing NatSpec

Every function should have a short textual description.

In addition to the textual description, there should be
a formal documentation for every parameter (@param).

***

Undocumented function:

    function _add(uint256 a, uint256 b) internal pure returns (uint256) {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L322-L322

***

Undocumented function:

    function _subtract(uint256 a, uint256 b) internal pure returns (uint256) {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L326-L326

***

Undocumented function:

    function transfersPaused(uint256 _data) internal pure returns (bool) {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol#L7-L7

***

Undocumented function:

    function mintingReservesPaused(uint256 _data) internal pure returns (bool) {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol#L11-L11

***

Undocumented function:

    function changingPricingResolverPaused(uint256 _data) internal pure returns (bool) {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol#L15-L15

***

Undocumented function:

    function decode(string memory _baseUri, bytes32 _hexString)
    external
    pure
    returns (string memory)
    {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L22-L26

***

Undocumented function:

    function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L66-L66

***

Undocumented function:

    function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L74-L74

***

Undocumented function:

    function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L82-L82


