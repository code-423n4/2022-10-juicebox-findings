# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1      | Immutable state variables lack zero address checks | Low | 5 |
| 2      | Setters should check the input value and revert if it's the zero address or zero | Low | 1 |
| 3      | Related data should be grouped in a struct |  NC | 10 |

## Findings

### 1- Immutable state variables lack zero address checks  :

Constructors should check the values written in an immutable state variables(address, uint, int) is not the zero value (address(0) or 0)

#### Impact - Low Risk

#### Proof of Concept
Instances include:

File: contracts/JBTiered721DelegateDeployer.sol [Line 51](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L51)
```
globalGovernance = _globalGovernance;
```

File: contracts/JBTiered721DelegateDeployer.sol [Line 52](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L52)
```
tieredGovernance = _tieredGovernance;
```

File: contracts/JBTiered721DelegateDeployer.sol [Line 53](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L53)
```
noGovernance = _noGovernance;
```

File: contracts/JBTiered721DelegateProjectDeployer.sol [Line 52](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L52)
```
controller = _controller;
```

File: contracts/JBTiered721DelegateProjectDeployer.sol [Line 53](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L53)
```
delegateDeployer = _delegateDeployer;
```

#### Mitigation
Add non-zero address check in the constructors for the instances aforementioned.

### 2- `setDefaultReservedTokenBeneficiary` missing zero address check for new default beneficiary :

The `setDefaultReservedTokenBeneficiary` function inside the `JBTiered721DelegateStore` contract is missing a zero address check for the new default beneficiary, this could lead to having `defaultReservedTokenBeneficiaryOf == address(0)` for a given collection which could be a problem if the `_reservedTokenBeneficiaryOf` is also address(0) because there will be no beneficiary for this collection.

#### Risk : Low

#### Proof of Concept

Instance include:

File: contracts/JBTiered721Delegate.sol [Line 375](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L370-L375)
```
function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {
    // Set the beneficiary.
    store.recordSetDefaultReservedTokenBeneficiary(_beneficiary);

    emit SetDefaultReservedTokenBeneficiary(_beneficiary, msg.sender);
  }
```

#### Mitigation
Add a non-zero address check in the `setDefaultReservedTokenBeneficiary` function

### 3- Related data should be grouped in a struct :

When there are mappings that use the same key value, having separate fields is error prone, for instance in case of deletion or with future new fields.

#### Impact - NON CRITICAL

#### Proof of Concept

Instances include:

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

#### Mitigation

Group the related data in a struct and use one mapping:

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
```

And it would be used as a state variable :

```
mapping(address => Collection) public nftCollections;
```
