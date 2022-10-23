# Gas Optimizations

## Findings

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | State variables only set in the constructor should be declared `immutable`  | 1 |
| 2      | Multiple address mappings can be combined into a single mapping of an address to a struct     | 10  |
| 3      | Use `unchecked` blocks to save gas  |  1 |
| 4      | `x += y`/`x -= y` costs more gas than `x = x + y`/`x = x - y` for state variables  |  1 |
| 5      | Use `calldata` instead of `memory` for function parameters type  |  12 |
| 6      | `++i/i++` should be `unchecked{++i}`/`unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops |  5  |

## Findings

### 1- State variables only set in the constructor should be declared `immutable` :

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

There are 4 instances of this issue:

File: contracts/JBTiered721Delegate.sol [Line 48](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L48)
```
address public override codeOrigin;
```

### 2- Multiple address mappings can be combined into a single mapping of an address to a struct :

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

There are 10 instances of this issue :

```
File: contracts/JBTiered721DelegateStore.sol

119      mapping(address => uint256) public override maxTierIdOf;
129      mapping(address => mapping(address => mapping(uint256 => uint256))) public override tierBalanceOf;
138      mapping(address => mapping(uint256 => uint256)) public override numberOfReservesMintedFor;
147      mapping(address => mapping(uint256 => uint256)) public override numberOfBurnedFor;
155      mapping(address => address) public override defaultReservedTokenBeneficiaryOf;
164      mapping(address => mapping(uint256 => address)) public override firstOwnerOf;
172      mapping(address => string) public override baseUriOf;  
180      mapping(address => IJBTokenUriResolver) public override tokenUriResolverOf;  
188      mapping(address => string) public override contractUriOf; 
197      mapping(address => mapping(uint256 => bytes32)) public override encodedIPFSUriOf;    
```

These mappings could be refactored into the following struct and mapping for example :

```
struct Collection {
    uint256 maxTierIdOf;
    address defaultReservedTokenBeneficiaryOf;
    string baseUriOf;
    string contractUriOf;
    IJBTokenUriResolver tokenUriResolverOf;
    mapping(address => mapping(uint256 => uint256)) tierBalanceOf;
    mapping(uint256 => uint256) numberOfReservesMintedFor;
    mapping(uint256 => uint256) numberOfBurnedFor;
    mapping(uint256 => address) firstOwnerOf;
    mapping(uint256 => bytes32) encodedIPFSUriOf;
}
    
mapping(address => Collection) public nftCollections;
```

### 3- Use `unchecked` blocks to save gas :

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block.

There is 1 instance of this issue:

File: contracts/JBTiered721DelegateStore.sol [Line 1077](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1077)

```
leftoverAmount = leftoverAmount - _storedTier.contributionFloor;
```

The above operation cannot underflow due to the check :

[if (_storedTier.contributionFloor > leftoverAmount) revert INSUFFICIENT_AMOUNT();](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1056) 

and should be modified as follows :

```
unchecked {
    leftoverAmount = leftoverAmount - _storedTier.contributionFloor;
}
```

### 4- `x += y/x -= y` costs more gas than `x = x + y/x = x - y` for state variables :

Using the addition operator instead of plus-equals saves **113 gas** as explained [here](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)

There is 1 instance of this issue:

File: contracts/JBTiered721DelegateStore.sol [Line 827](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L827)
```
numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

### 5- Use `calldata` instead of `memory` for function parameters type :

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

There are 12 instances of this issue:

File: contracts/JB721TieredGovernance.sol [Line 147](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L147)
```
function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)
```

File: contracts/JBTiered721Delegate.sol [Line 264](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L264)
```
function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)
```

File: contracts/JBTiered721Delegate.sol [Line 290](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290)
```
function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)
```

File: contracts/JBTiered721Delegate.sol [Line 490](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L490)
```
function mintFor(uint16[] memory _tierIds, address _beneficiary)
```

File: contracts/JBTiered721Delegate.sol [Line 598](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L598)
```
function _didBurn(uint256[] memory _tokenIds) internal override
```

File: contracts/JBTiered721Delegate.sol [Line 650-654](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L650-L654)
```
function _mintAll(
    uint256 _amount,
    uint16[] memory _mintTierIds,
    address _beneficiary
  )
```

File: contracts/JBTiered721Delegate.sol [Line 695](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L695)
```
function _redemptionWeightOf(uint256[] memory _tokenIds)
```

File: contracts/JBTiered721DelegateStore.sol [Line 628](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L628)
```
function recordAddTiers(JB721TierParams[] memory _tiersToAdd)
```

File: contracts/JBTiered721DelegateStore.sol [Line 1091](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1091)
```
function recordBurn(uint256[] memory _tokenIds)
```

File: contracts/libraries/JBIpfsDecoder.sol [Line 628](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L66)
```
function _truncate(uint8[] memory _array, uint8 _length)
```

File: contracts/libraries/JBIpfsDecoder.sol [Line 74](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L74)
```
function _reverse(uint8[] memory _input)
```

File: contracts/libraries/JBIpfsDecoder.sol [Line 82](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L82)
```
function _toAlphabet(uint8[] memory _indices)
```

### 6- `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops :

There are 5 instances of this issue:

File: contracts/libraries/JBIpfsDecoder.sol [Line 49](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49)
```
for (uint256 i = 0; i < _source.length; ++i)
```

File: contracts/libraries/JBIpfsDecoder.sol [Line 51](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L51)
```
for (uint256 j = 0; j < digitlength; ++j)
```

File: contracts/libraries/JBIpfsDecoder.sol [Line 68](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68)
```
for (uint256 i = 0; i < _length; i++)
```

File: contracts/libraries/JBIpfsDecoder.sol [Line 76](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)
```
for (uint256 i = 0; i < _input.length; i++)
```

File: contracts/libraries/JBIpfsDecoder.sol [Line 84](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)
```
for (uint256 i = 0; i < _indices.length; i++)
```