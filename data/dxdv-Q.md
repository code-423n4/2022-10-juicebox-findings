# Findings for Juicebox Contest
**Commit:** `N/A`
**Type of Audit:** Security Review
**Type of Project:** NFT Reward Mechanism
**Language**: Solidity
**Methods**: Manual review

---

## 1. Contract `JB721GlobalGovernance`


### Unimported `JB721Tier` struct 

**File**: [L55](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L55) 

**Description**: Whereas `JB721Tier` struct is passed as parameter in `_afterTokenTransferAccounting` function, it is not imported in `JB721GlobalGovernance.sol` 

**Recommendation**: Consider importing `JB721Tier` struct in `JB721GlobalGovernance.sol` 


&nbsp;
---
## 2. Contract `JBTiered721Delegate.sol`

### Unimported `JBTiered721MintForTiersData` struct 

**File**: [L290](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290-L309) 

**Description**: `JBTiered721MintForTiersData` struct is not imported in `JBTiered721Delegate.sol` contract; however, it is referrenced  as `JBTiered721MintForTiersData[] memory _mintForTiersData` parameter in `mintFor` function


**Recommendation**: Consider importing `JBTiered721MintForTiersData` struct in `JBTiered721Delegate.sol` 




&nbsp;
---
## 3. Contract `JBTiered721DelegateStore.sol`

### Unimported `JB721Tier` and `JB721TierParams` structs

**File**: [L216](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L217) [L279](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L279) [L628](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L628)

**Description**: While `JB721Tier` struct is not imported in `JBTiered721DelegateStore.sol` contract, it is referenced as returned value `JB721Tier[] memory _tiers` for `tiers` function,  and as the returned value`JB721Tier[] memory` for `tier` function

`JB721TierParams` is not imported but it is referrenced  as `JB721TierParams[] memory _tiersToAdd` parameter in `recordAddTiers` function


**Recommendation**: Consider importing `JB721Tier` and `JB721TierParams` structs in `JBTiered721DelegateStore.sol` 

---


### Missing emitted event in state-changing `recordTransferForTier` function
**File**: [L866](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L866-L883) 
```solidity=
function recordTransferForTier(
    uint256 _tierId,
    address _from,
    address _to
  ) external override {
    // If this is not a mint then subtract the tier balance from the original holder.
    if (_from != address(0))
      // decrease the tier balance for the sender
      --tierBalanceOf[msg.sender][_from][_tierId];

    // if this is a burn the balance is not added
    if (_to != address(0)) {
      unchecked {
        // increase the tier balance for the beneficiary
        ++tierBalanceOf[msg.sender][_to][_tierId];
      }
    }
}

```
**Description**: `recordTransferForTier` function does not emit any event. 

**Recommendation**: Consider creating and emitting appropriate event for `recordTransferForTier` function

---

### Missing emitted event for state-changing `recordSetDefaultReservedTokenBeneficiary` function
**File**: [L854](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L854-L856) 
```solidity=
function recordSetDefaultReservedTokenBeneficiary(address _beneficiary) external override {
    defaultReservedTokenBeneficiaryOf[msg.sender] = _beneficiary;
}
```
**Description**: State-changing `recordSetDefaultReservedTokenBeneficiary` function does not emit any event. 

**Recommendation**: Consider creating and emitting appropriate event for `recordSetDefaultReservedTokenBeneficiary` function

---

### Missing zero address validation logic in `recordSetDefaultReservedTokenBeneficiary` function

**File**: [L854](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L854-L856) 

```solidity=
 function recordSetDefaultReservedTokenBeneficiary(address _beneficiary) external override {
    defaultReservedTokenBeneficiaryOf[msg.sender] = _beneficiary;
}
```

**Description**: `recordSetDefaultReservedTokenBeneficiary` lacks validation logic to ensure that `_beneficiary` param of `recordSetDefaultReservedTokenBeneficiary` function is not a zero address


**Recommendation**: Consider adding validation logic to check zero address parameter



&nbsp;
---
## 4. Contract `JB721TieredGovernance`

### No emitted event in `setTierDelegates` function
**File**: [L147](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L147-L168) 
```solidity=
function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)
    public
    virtual
    override
  {
    // Keep a reference to the number of tier delegates.
    uint256 _numberOfTierDelegates = _setTierDelegatesData.length;

    // Keep a reference to the data being iterated on.
    JBTiered721SetTierDelegatesData memory _data;

    for (uint256 _i; _i < _numberOfTierDelegates; ) {
      // Reference the data being iterated on.
      _data = _setTierDelegatesData[_i];

      _delegateTier(msg.sender, _data.delegatee, _data.tierId);

      unchecked {
        ++_i;
      }
    }
 }

```
**Description**: `setTierDelegates` function does not emit any event. 

**Recommendation**: Consider creating and emitting appropriate event for `setTierDelegates` function


