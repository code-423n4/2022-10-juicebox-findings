### L-01 \_SAFEMINT() SHOULD BE USED RATHER THAN \_MINT() WHEREVER POSSIBLE

`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
461: _mint(_reservedTokenBeneficiary, _tokenId);
504: _mint(_beneficiary, _tokenId);
635: _mint(_beneficiary, _tokenId);
677: _mint(_beneficiary, _tokenId);
```

### L-02 REPLACE INLINE ASSEMBLY WITH ACCOUNT.CODE.LENGTH

`<address>.code.length` can be used in Solidity >= 0.8.0 to access an account’s code size and check if it is a contract without inline assembly.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol


```
let _codeSize := extcodesize(_targetAddress)
```


### N-01 REQUIRE() STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
216: require(address(this) != codeOrigin);
218: require(address(store) == address(0));
```

### N-02 FLOATING PRAGMA VERSION SHOULD NOT BE USED


This is applicable accross all the smart contracts.


### N-03 NATSPEC IS INCOMPLETE

This is applicable accross all contracts, here are few instances:

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
44:  function _toBase58(bytes memory _source) private pure returns (string memory) {
66:  function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
```
