## L-01 _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE

`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.

### Proof of Concept

There are 5 instances of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
File: contracts/JBTiered721Delegate.sol

461: _mint(_reservedTokenBeneficiary, _tokenId);
504: _mint(_beneficiary, _tokenId);
635: _mint(_beneficiary, _tokenId);
677: _mint(_beneficiary, _tokenId);
```
---------

## N-01 REQUIRE() OR REVERT() STATEMENTS SHOULD HAVE DESCRIPTIVE REASON STRINGS

There are 2 instances of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
File: contracts/JBTiered721Delegate.sol

216: require(address(this) != codeOrigin);
218: require(address(store) == address(0));
```

--------------

## N-02 NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Proof of Concept

This issue exists on all In-scope contracts

### Recommended Mitigation Steps

Lock the pragma version

------------
