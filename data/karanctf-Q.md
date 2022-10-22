#Low impact

## [L-01] Use of `Block.timestamp`
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
```solidity
JBTiered721DelegateStore.sol:903:      if (_storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();
```
## [L-02] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
```solidity
JBIpfsDecoder.sol-24-    pure
JBIpfsDecoder.sol-25-    returns (string memory)
JBIpfsDecoder.sol-26-  {
JBIpfsDecoder.sol-27-    // Concatenate the hex string with the fixed IPFS hash part (0x12 and 0x20)
JBIpfsDecoder.sol:28:    bytes memory completeHexString = abi.encodePacked(bytes2(0x1220), _hexString);
JBIpfsDecoder.sol-29-
JBIpfsDecoder.sol-30-    // Convert the hex string to an hash
JBIpfsDecoder.sol-31-    string memory ipfsHash = _toBase58(completeHexString);
JBIpfsDecoder.sol-32-
JBIpfsDecoder.sol-33-    // Concatenate with the base URI
JBIpfsDecoder.sol:34:    return string(abi.encodePacked(_baseUri, ipfsHash));
JBIpfsDecoder.sol-35-  }
JBIpfsDecoder.sol-36-
JBIpfsDecoder.sol-37-  /**
JBIpfsDecoder.sol-38-    @notice
```








# Non critical
## [N-1] `require()`/`revert()` statements should have descriptive reason strings
```solidity
JBTiered721Delegate.sol:216:    require(address(this) != codeOrigin);
JBTiered721Delegate.sol:218:    require(address(store) == address(0));
```
## [N-2] Try making public function internal if there is no external use such functions often include division multiply round etc
```solidity
JBTiered721Delegate.sol:123:  function balanceOf(address _owner) public view override returns (uint256 balance) {
JBTiered721Delegate.sol:138:  function tokenURI(uint256 _tokenId) public view override returns (string memory) {
JBTiered721Delegate.sol:175:  function supportsInterface(bytes4 _interfaceId) public view override returns (bool) {
JBTiered721DelegateStore.sol:550:  function totalRedemptionWeight(address _nft) public view override returns (uint256 weight) {
JBTiered721DelegateStore.sol:585:  function tierIdOfToken(uint256 _tokenId) public pure override returns (uint256) {
```

ch
