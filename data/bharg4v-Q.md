# Quality Assurance Report

## Issues 


|   |      Issue     |  Instances |
|----------|-------------|:------:|
| 1 | Missing checks for address(0x0) when assigning values to address state variables | 3 |
| 2  |_safeMint() should be used rather than _mint() wherever possible | 4 |


### 1. Missing checks for address(0x0) when assigning values to address state variables
Zero address should be checked for state variables and some parameters in functions like mints, withdrawals... A zero address can lead into problems.

### POC 

```solidity
220┆ _tierDelegation[_account][_tierId] = _delegatee; 
```
### Instances
```
code4rena/2022-10-juicebox/contracts2/JB721TieredGovernance.sol L220
code4rena/2022-10-juicebox/contracts2/JBTiered721DelegateStore.sol 855
code4rena/2022-10-juicebox/contracts2/JBTiered721DelegateStore.sol 1124
```

### Mitigation 
Check zero address for state variables before assigning to them a value





### 2. _safeMint() should be used rather than _mint() wherever possible

_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both open OpenZeppelin and solmate have versions of this function so that NFTs aren’t lost if they’re minted to contracts that cannot transfer them back out.

### POC 

```solidity
461┆ _mint(_reservedTokenBeneficiary, _tokenId); 
```
### Instances
```
code4rena/2022-10-juicebox/contracts2/JBTiered721Delegate.sol L461
code4rena/2022-10-juicebox/contracts2/JBTiered721Delegate.sol L504
code4rena/2022-10-juicebox/contracts2/JBTiered721Delegate.sol L635
code4rena/2022-10-juicebox/contracts2/JBTiered721Delegate.sol L677
```
### Mitigation
 “Unless there is a compelling reason, abi.encode should be preferred”. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.



