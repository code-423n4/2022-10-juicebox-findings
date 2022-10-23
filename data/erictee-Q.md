### [L-01] Avoid floating pragmas for non-library contracts.


#### Impact
While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### Findings:
```
contracts/JB721GlobalGovernance.sol:L2     pragma solidity ^0.8.16;

contracts/JBTiered721DelegateProjectDeployer.sol:L2     pragma solidity ^0.8.16;

contracts/JB721TieredGovernance.sol:L2     pragma solidity ^0.8.16;

contracts/JBTiered721DelegateStore.sol:L2     pragma solidity ^0.8.16;

contracts/JBTiered721DelegateDeployer.sol:L2     pragma solidity ^0.8.16;

contracts/JBTiered721Delegate.sol:L2     pragma solidity ^0.8.16;

contracts/abstract/JB721Delegate.sol:L2     pragma solidity ^0.8.16;

```

### [L-02] ```require()```/```revert()``` statements should have descriptive strings.


#### Impact
Consider adding descriptive strings in ```require()```/```revert()```.


#### Findings:
```
contracts/JBTiered721Delegate.sol:L216    require(address(this) != codeOrigin);

contracts/JBTiered721Delegate.sol:L218    require(address(store) == address(0));

```

### [L-03] ```_safemint()``` should be used rather than ```_mint()``` wherever possible


#### Impact
```_mint()``` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of ```_safeMint()``` which ensures that the recipient is either an EOA or implements ```IERC721Receiver```. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function


#### Findings:
```
contracts/JBTiered721Delegate.sol:L461      _mint(_reservedTokenBeneficiary, _tokenId);

contracts/JBTiered721Delegate.sol:L504      _mint(_beneficiary, _tokenId);

contracts/JBTiered721Delegate.sol:L635    _mint(_beneficiary, _tokenId);

contracts/JBTiered721Delegate.sol:L677      _mint(_beneficiary, _tokenId);

```
