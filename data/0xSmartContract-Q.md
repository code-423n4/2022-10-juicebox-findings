### Non-Critical Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
| [NC-01 ]| Insufficient coverage | |
| [NC-02] |`0` address check |4|
| [NC-03] |For modern and more readable code; update import usages | All contracts |
| [NC-04] | `Function writing` that does not comply with the `Solidity Style Guide` | 3 |
| [NC-05] | State variables should be marked `immutable` | 1 |
| [NC-06] | Omissions in Events | 4 |
| [NC-07] |Need Fuzzing test | 7 |
| [NC-08] | Use a more recent version of Solidity | All contracts |

Total 8 issues

### Low Risk Issues List
| Number |Issues Details|Context|
|:--:|:-------|:--:|
|[L-01]| Use ```safeTransferOwnership``` instead of ```transferOwnership``` function | 2 |
|[L-02]|Use a more recent version of OpenZeppelin dependencies| All contracts |

Total 2 issues

### Suggestions
| Number | Suggestion Details |
|:--:|:-------|
| [S-01] |Add to _blacklist function_ |

Total 1 suggestion


### [N-01] Insufficient coverage

**Description:**
The test coverage rate of the project is 30%. Testing all functions is best practice in terms of security criteria.

```js
+-----------------------+-------------------+-------------------+-------------------+------------------+
| File                  | % Lines           | % Statements      | % Branches        | % Funcs          |
+======================================================================================================+
| Total                 | 33.35% (790/2369) | 33.75% (947/2806) | 22.86% (267/1168) | 30.35% (163/537) |
+-----------------------+-------------------+-------------------+-------------------+------------------+
```
Due to its capacity, test coverage is expected to be 100%.


### [N-02] `0 address` check

**Context:**
[JBTiered721DelegateDeployer.sol#L51-L53](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L51-L53)
[JBTiered721DelegateProjectDeployer.sol#L52-L53](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L52-L53)
[JB721Delegate.sol#L212](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L212)
[JBTiered721Delegate.sol#L223-L224](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L223-L224)

**Description:**
Also check of the address to protect the code from 0x0 address  problem just in case. This is best practice or instead of suggesting that they verify address != 0x0, you could add some good NatSpec comments explaining what is valid and what is invalid and what are the implications of accidentally using an invalid address.

**Recommendation:**
like this;
`if (routerV2== address(0)) revert ADDRESS_ZERO();`


### [N-03] For modern and more readable code; update import usages

**Context:**
All contracts

**Description:**
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct `polluted the source code` with an unnecessary object we were not using because we did not need it. 
This was breaking the rule of modularity and modular programming: `only import what you need` Specific imports with curly braces allow us to apply this rule better.

**Recommendation:**
`import {contract1 , contract2} from "filename.sol";`

A good example from the ArtGobblers project;
```js
import {Owned} from "solmate/auth/Owned.sol";
import {ERC721} from "solmate/tokens/ERC721.sol";
import {LibString} from "solmate/utils/LibString.sol";
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```


### [N-04] `Function writing` that does not comply with the `Solidity Style Guide`

**Context:**
[JBTiered721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol)
[JBTiered721DelegateStore.sol](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol)
[JB721Delegate.sol](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol)

**Description:**
Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private
- within a grouping, place the view and pure functions last


### [N-05] State variables should be marked `immutable`

**Context:**
[JB721Delegate.sol#L48-LL62](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L48-LL62)


**Recommendation:**
The `projectId` and `directory` state variables should be changed to `immutable`, also adhering to the Natspec comments.


### [N-06] Omissions in Events

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters

The events should include the new value and old value where possible:
Events with no old value;;
[JBTiered721Delegate.sol#L374](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L374)
[JBTiered721Delegate.sol#L390](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L390)
[JBTiered721Delegate.sol#L406](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L406)
[JBTiered721Delegate.sol#L422](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L422)


### [N-07] Need Fuzzing test

**Context:**
22 results - 4 files
Project uncheckeds list:

```js
contracts\JB721TieredGovernance.sol:
   164:       unchecked {
  165          ++_i;
contracts\JBTiered721Delegate.sol:
  278:       unchecked {
  279          ++_i;
 305:       unchecked {
  306          ++_i;  339          emit RemoveTier(_tierIdsToRemove[_i], msg.sender);
  340:         unchecked {
  341            ++_i;
  353          emit AddTier(_tierIdsAdded[_i], _tiersToAdd[_i], msg.sender);
  354:         unchecked {
  355            ++_i;
  465:       unchecked {
  466          ++_i;
  508:       unchecked {
  509          ++_i;
  681:       unchecked {
  682          ++_i;
contracts\JBTiered721DelegateStore.sol:
   356:       unchecked {
   357          --_i;
   411:       unchecked {
   412          --_i;
   508:       unchecked {
   509          --_i;
   536:       unchecked {
   537          ++_i;
   568:       unchecked {
   569          ++_i;
   788:       unchecked {
   789          ++_i;
   842:       unchecked {
   843          ++_i;
   877      if (_to != address(0)) {
   878:       unchecked {
   879          // increase the tier balance for the beneficiary
   908:       unchecked {
   909          ++_i;
   985        // Make the token ID.
   986:       unchecked {
   987          // Keep a reference to the token ID.
  1065        // Mint the tokens.
  1066:       unchecked {
  1067          // Keep a reference to the token ID.
  1079:       unchecked {
  1080          ++_i;
  1110:       unchecked {
  1111          ++_i;
contracts\abstract\JB721Delegate.sol:
  281  
  282:       unchecked {
  283          ++_i;
```

**Description:**
In total 4 contracts, 22 unchecked are used, the functions used are critical. For this reason, there must be fuzzing tests in the tests of the project. Not seen in tests.

**Recommendation:**
Use should fuzzing test like Echidna.

As Alberto Cuesta Canada said:
_Fuzzing is not easy, the tools are rough, and the math is hard, but it is worth it. Fuzzing gives me a level of confidence in my smart contracts that I didn’t have before. Relying just on unit testing anymore and poking around in a testnet seems reckless now._

https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05


### [N-08] Use a more recent version of Solidity

**Context:**
All contracts

**Description:**
For security, it is best practice to use the latest Solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md


**Recommendation:**
Old version of Solidity is used `(^0.8.16)`, newer version can be used `(0.8.17)` 


### [L-01] Use ```safeTransferOwnership``` instead of ```transferOwnership``` function

**Context:**
[JBTiered721Delegate.sol#L251](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L251)
[JBTiered721DelegateDeployer.sol#L100](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L100)

**Description:**
```transferOwnership``` function is used to change Ownership

Use a 2 structure transferOwnership which is safer. 
```safeTransferOwnership```,  use it is more secure due to 2-stage ownership transfer.

**Recommendation:**
Use ``Ownable2Step.sol``
[Ownable2Step.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)

```js
 /**
     * @dev The new owner accepts the ownership transfer.
     */
    function acceptOwnership() external {
        address sender = _msgSender();
        require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
        _transferOwnership(sender);
    }
}
```


### [L-02] Use a more recent version of OpenZeppelin dependencies

**Context:**
All contracts

**Description:**
For security, it is best practice to use the latest OZ version.

[package.json#L14](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/package.json#L14)

```js
"name": "@openzeppelin/contracts",
"description": "Secure Smart Contract library for Solidity",
"version": "^4.6.0",
```

For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md


**Recommendation:**
Old version of OZ is used `(4.6.0)`, newer version can be used `(4.7.3)` 

https://github.com/OpenZeppelin/openzeppelin-contracts/releases/tag/v4.7.3

### [S-01] Add to _blacklist function_

**Description:**
Cryptocurrency mixing service, Tornado Cash, has been blacklisted in the OFAC.
A lot of blockchain companies, token projects, NFT Projects have ```blacklisted``` all Ethereum addresses owned by Tornado Cash listed in the US Treasury Department's sanction against the protocol.
https://home.treasury.gov/policy-issues/financial-sanctions/recent-actions/20220808

Some of these Projects;
 USDC 
 Aave 
 Uniswap
 Balancer
 Infura
 Alchemy 
 Opensea
 dYdX 

Transactions from the project by an account funded by Tonadocash or banned by OFAC can lead to legal problems.Especially American citizens may want to get addresses to the blacklist legally, this is not an obligation

If you think that such legal prohibitions should be made directly by validators, this may not be possible:
https://www.paradigm.xyz/2022/09/base-layer-neutrality

```The ban on Tornado Cash makes little sense, because in the end, no one can prevent people from using other mixer smart contracts, or forking the existing ones. It neither hinders cybercrime, nor privacy.```

Here is the most beautiful and close to the project example; Manifold

Manifold Contract
https://etherscan.io/address/0xe4e4003afe3765aca8149a82fc064c0b125b9e5a#code

```js
     modifier nonBlacklistRequired(address extension) {
         require(!_blacklistedExtensions.contains(extension), "Extension blacklisted");
         _;
     }
```
Recommended Mitigation Steps add to Blacklist function and modifier.





