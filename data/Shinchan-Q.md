
# **Low and Non-Critical**

Serial No. | Issue Name | Instances
--- | --- | ---
Q-1 | _safeMint() should be used rather than _mint() wherever possible | 4
Q-2 | Use of Block.Timestamp | 1


-----
***Total instances found in this contest: 5 | Among all files in scope***


## Q-01 _safeMint() should be used rather than _mint() wherever possible

`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. Both open [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/Rari-Capital/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.

### Instances
[contracts/JBTiered721Delegate.sol:461:      _mint(_reservedTokenBeneficiary, _tokenId);](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L461)
[contracts/JBTiered721Delegate.sol:504:      _mint(_beneficiary, _tokenId);](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L504)
[contracts/JBTiered721Delegate.sol:635:    _mint(_beneficiary, _tokenId);](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L635)
[contracts/JBTiered721Delegate.sol:677:      _mint(_beneficiary, _tokenId);](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L677)
### Recommendations:

Use _safeMint() instead of _mint().


-----

## Q-02 Use of Block.Timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

### Instances:
[contracts/JBTiered721DelegateStore.sol:903:      if (_storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L903)
### References:

https://github.com/code-423n4/2022-04-dualityfocus-findings/issues/33


-----

