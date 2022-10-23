## QA Issues found

### [L-01] Unspecific Compiler Version Pragma
#### Impact
A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.
#### Findings:
```solidity
juice-nft-rewards\JB721Delegate.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JB721GlobalGovernance.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JB721TieredGovernance.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBBitmap.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBIpfsDecoder.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721Delegate.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721DelegateDeployer.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721DelegateProjectDeployer.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721DelegateStore.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards\JBTiered721FundingCycleMetadataResolver.sol::2 => pragma solidity ^0.8.16;
```
#### Recommendation
Avoid floating pragmas for non-library contracts. It is recommended to pin to a concrete compiler version.

### [L-02] `_safeMint()` should be used rather than `_mint()` wherever possible.
#### Impact
`_mint()` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`.
#### Findings:
```solidity
juice-nft-rewards\JBTiered721Delegate.sol::461 => _mint(_reservedTokenBeneficiary, _tokenId);
juice-nft-rewards\JBTiered721Delegate.sol::504 => _mint(_beneficiary, _tokenId);
juice-nft-rewards\JBTiered721Delegate.sol::635 => _mint(_beneficiary, _tokenId);
juice-nft-rewards\JBTiered721Delegate.sol::677 => _mint(_beneficiary, _tokenId);
```
#### Recommendation
Use either [OpenZeppelin's](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) or [solmate's](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) version of this function.
