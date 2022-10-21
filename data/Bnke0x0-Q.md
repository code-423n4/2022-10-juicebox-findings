


### [L01] Missing checks for `address(0x0)` when assigning values to `address` state variables


#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::223 => fundingCycleStore = _fundingCycleStore;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::224 => store = _store;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::225 => pricingCurrency = _pricing.currency;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::226 => pricingDecimals = _pricing.decimals;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::227 => prices = _pricing.prices;
juice-nft-rewards/contracts/JBTiered721DelegateDeployer.sol::51 => globalGovernance = _globalGovernance;
juice-nft-rewards/contracts/JBTiered721DelegateDeployer.sol::52 => tieredGovernance = _tieredGovernance;
juice-nft-rewards/contracts/JBTiered721DelegateDeployer.sol::53 => noGovernance = _noGovernance;
juice-nft-rewards/contracts/JBTiered721DelegateProjectDeployer.sol::52 => controller = _controller;
juice-nft-rewards/contracts/JBTiered721DelegateProjectDeployer.sol::53 => delegateDeployer = _delegateDeployer;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::751 => _startSortIndex = _currentSortIndex;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1276 => tokenId = _tierId;
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::211 => projectId = _projectId;
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::212 => directory = _directory;
```



### [L02] `initialize` functions can be front-run

#### Impact
See [this](https://github.com/code-423n4/2021-10-badgerdao-findings/issues/40) finding from a prior badger-dao contest for details
#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::202 => function initialize(
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::203 => function _initialize(
```



### [L03] `_safeMint()` should be used rather than `_mint()` wherever possible

#### Impact
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and solmate have versions of this function
#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::461 => _mint(_reservedTokenBeneficiary, _tokenId);
juice-nft-rewards/contracts/JBTiered721Delegate.sol::504 => _mint(_beneficiary, _tokenId);
juice-nft-rewards/contracts/JBTiered721Delegate.sol::635 => _mint(_beneficiary, _tokenId);
juice-nft-rewards/contracts/JBTiered721Delegate.sol::677 => _mint(_beneficiary, _tokenId);
```



### [L04] Unspecific Compiler Version Pragma

#### Impact
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.
#### Findings:
```
juice-nft-rewards/contracts/JB721GlobalGovernance.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JB721TieredGovernance.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JBTiered721DelegateDeployer.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JBTiered721DelegateProjectDeployer.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/libraries/JBBitmap.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol::2 => pragma solidity ^0.8.16;
```



### *Non-Critical Issues*




### [N01] `require()`/`revert()` statements should have descriptive reason strings


#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::216 => require(address(this) != codeOrigin);
juice-nft-rewards/contracts/JBTiered721Delegate.sol::218 => require(address(store) == address(0));
```



### [N02] constants should be defined rather than using magic numbers


#### Findings:
```
juice-nft-rewards/contracts/JBTiered721Delegate.sol::531 => 10**pricingDecimals,
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::52 => carry += uint256(digits[j]) * 256;
```



### [N03] Use bit shifts in an imutable variable rather than long bit masks of a single bit, for readability


#### Findings:
```
juice-nft-rewards/contracts/JBTiered721DelegateDeployer.sol::124 => let _mask := mul(_codeSize, 0x100000000000000000000000000000000000000000000000000000000)
```





### [N04] Unused file


#### Findings:
```
juice-nft-rewards/contracts/JB721GlobalGovernance.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/JB721TieredGovernance.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/JBTiered721Delegate.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/JBTiered721DelegateDeployer.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/JBTiered721DelegateProjectDeployer.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/libraries/JBBitmap.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::1 => // SPDX-License-Identifier: MIT
juice-nft-rewards/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol::1 => // SPDX-License-Identifier: MIT
```


### [N05] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external .

#### Findings:
```
juice-nft-rewards/contracts/JB721TieredGovernance.sol::177 => function setTierDelegate(address _delegatee, uint256 _tierId) public virtual override {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::123 => function balanceOf(address _owner) public view override returns (uint256 balance) {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::138 => function tokenURI(uint256 _tokenId) public view override returns (string memory) {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::175 => function supportsInterface(bytes4 _interfaceId) public view override returns (bool) {
juice-nft-rewards/contracts/JBTiered721Delegate.sol::436 => function mintReservesFor(uint256 _tierId, uint256 _count) public override {
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::499 => function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) {
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::523 => function redemptionWeightOf(address _nft, uint256[] calldata _tokenIds)
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::524 => public
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::550 => function totalRedemptionWeight(address _nft) public view override returns (uint256 weight) {
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::585 => function tierIdOfToken(uint256 _tokenId) public pure override returns (uint256) {
```



### [N06] NC-library/interface files should use fixed compiler versions, not floating ones


#### Findings:
```
juice-nft-rewards/contracts/JB721GlobalGovernance.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JB721TieredGovernance.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JBTiered721Delegate.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JBTiered721DelegateDeployer.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JBTiered721DelegateProjectDeployer.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/JBTiered721DelegateStore.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/abstract/JB721Delegate.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/libraries/JBBitmap.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/libraries/JBIpfsDecoder.sol::2 => pragma solidity ^0.8.16;
juice-nft-rewards/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol::2 => pragma solidity ^0.8.16;
```






