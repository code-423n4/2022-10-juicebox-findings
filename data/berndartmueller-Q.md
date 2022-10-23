## Table of Contents

- [Low Risk](#low-risk)
  - [[L-01] `JBTiered721Delegate.tokenURI` should throw an error if `_tokenId` is not a valid NFT](#l-01-jbtiered721delegatetokenuri-should-throw-an-error-if-_tokenid-is-not-a-valid-nft)
  - [[L-02] Decoding an IPFS hash is using a fixed hash function and length of hash](#l-02-decoding-an-ipfs-hash-is-using-a-fixed-hash-function-and-length-of-hash)
  - [[L-03] The tier id can potentially surpass 16 bits leading to token id collisions](#l-03-the-tier-id-can-potentially-surpass-16-bits-leading-to-token-id-collisions)

## Low Risk

### [L-01] `JBTiered721Delegate.tokenURI` should throw an error if `_tokenId` is not a valid NFT

#### Description

According to [`EIP-721`](https://eips.ethereum.org/EIPS/eip-721) and specifically, the metadata extension, the `tokenURI` function should throw an error if `_tokenId` is not a valid NFT. Contrary, the current implementation returns an empty string.

#### Findings

[JBTiered721Delegate.sol#L140](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L140)

```solidity
function tokenURI(uint256 _tokenId) public view override returns (string memory) {
  // A token without an owner doesn't have a URI.
  if (_owners[_tokenId] == address(0)) return ''; // @audit-info Should throw instead of returning an empty string

  // Get a reference to the URI resolver.
  IJBTokenUriResolver _resolver = store.tokenUriResolverOf(address(this));

  // If a token URI resolver is provided, use it to resolve the token URI.
  if (address(_resolver) != address(0)) return _resolver.getUri(_tokenId);

  // Return the token URI for the token's tier.
  return
    JBIpfsDecoder.decode(
      store.baseUriOf(address(this)),
      store.encodedTierIPFSUriOf(address(this), _tokenId)
    );
}
```

#### Recommended mitigation steps

Consider throwing an error if `_tokenId` is not a valid NFT.

### [L-02] Decoding an IPFS hash using a fixed hash function and length of the hash

#### Description

An IPFS hash specifies the hash function and length of the hash in the first two bytes of the hash. The first two bytes are **0x1220**, where **12** denotes that this is the SHA256 hash function and **20** is the length of the hash in bytes (32 bytes).

Although SHA256 is 32 bytes and is currently the most common IPFS hash function, other content could use a hash function that is larger than 32 bytes. The current implementation limits the usage to the SHA256 hash function and a hash length of 32 bytes.

#### Findings

[libraries/JBIpfsDecoder.sol#L28](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L28)

```solidity
function decode(string memory _baseUri, bytes32 _hexString)
  external
  pure
  returns (string memory)
{
  // Concatenate the hex string with the fixed IPFS hash part (0x12 and 0x20)
  bytes memory completeHexString = abi.encodePacked(bytes2(0x1220), _hexString);

  // Convert the hex string to an hash
  string memory ipfsHash = _toBase58(completeHexString);

  // Concatenate with the base URI
  return string(abi.encodePacked(_baseUri, ipfsHash));
}
```

#### Recommended mitigation steps

Consider using a more generic implementation that can handle different hash functions and lengths and allow the user to choose.

### [L-03] The tier id can potentially surpass 16 bits leading to token id collisions

#### Description

The token id is composed of the given tier id `_tierId` and the number of the token `_tokenNumber` in the tier. The tier id is limited to 16 bits, which means that there can **theoretically** only exist 65,535 tiers _(this is very unlikely as this would have more serious consequences on other parts of the system and will cause a serious denial of service caused by unbounded loops. Still, theoretically, it's possible and there is no check in place)_.

If more than 65,535 tiers exist, the 16 bits reserved for the tier id will be surpassed and overwritten by `_tokenNumber`. This will lead to token id collisions with other tiers with a lower tier id.

#### Findings

[JBTiered721DelegateStore.\_generateTokenId](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1276)

```solidity
function _generateTokenId(uint256 _tierId, uint256 _tokenNumber)
  internal
  pure
  returns (uint256 tokenId)
{
  // The tier ID in the first 16 bits.
  tokenId = _tierId;

  // The token number in the rest.
  tokenId |= _tokenNumber << 16;
}
```

#### Recommended mitigation steps

Consider reverting if the `_tierId` is > 16 bits.
