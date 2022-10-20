### Contracts include unused custom errors

Locations:
[1](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L36)
[2](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L37)

#### Description

Unused errors simply drive up the cost of deployment for the contracts involved. These unused errors may also represent holes in logic that were intended to be tested but were missed. 

Ensure that the errors are unnecessary before removing them.

### Use `calldata` when possible over `memory` for function arguments

Locations:
[1](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L72)
[2](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L109)
[3](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L152)
[4](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290)
[5](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L386)
[6](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L628)
[7](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1091)
[8](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L147)
[9](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L22)
[10](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L210-L213) <-- This breaks a test in NFTReward_Unit.t.sol due to the use of a constructor to build arguments to call this initializer


#### Description

While this codebase already makes some use of `calldata` for function arguments, it can be used more pervasively than it is.

#### Savings

Output of `forge snapshot --diff` after changing just 3 instances of `memory` to `calldata` in `JBTiered721DelegateProjectDeployer.sol`

```
testMintAndTransferTieredVotingUnits(uint8,bool) (gas: -5658 (-0.062%)) 
testMintAndDelegateVotingUnits(uint8,bool) (gas: -5712 (-0.063%)) 
testRedeemAll() (gas: -7572 (-0.083%)) 
testRedeemAll() (gas: -7572 (-0.083%)) 
testRedeemAll() (gas: -7572 (-0.083%)) 
testMintAndDelegateTieredVotingUnits(uint8,bool) (gas: -7572 (-0.083%)) 
testMintAndTransferGlobalVotingUnits(uint8,bool) (gas: -7572 (-0.084%)) 
testMintOnPayIfMultipleTiersArePassed() (gas: -7572 (-0.085%)) 
testMintOnPayIfMultipleTiersArePassed() (gas: -7572 (-0.085%)) 
testMintOnPayIfMultipleTiersArePassed() (gas: -7572 (-0.085%)) 
testMintBeforeAndAfterTierChange(uint72) (gas: -7572 (-0.086%)) 
testMintBeforeAndAfterTierChange(uint72) (gas: -7572 (-0.086%)) 
testMintBeforeAndAfterTierChange(uint72) (gas: -7572 (-0.086%)) 
testMintReservedToken() (gas: -7572 (-0.087%)) 
testMintReservedToken() (gas: -7572 (-0.087%)) 
testMintReservedToken() (gas: -7572 (-0.087%)) 
testRedeemToken(uint16) (gas: -7572 (-0.087%)) 
testRedeemToken(uint16) (gas: -7572 (-0.087%)) 
testRedeemToken(uint16) (gas: -7572 (-0.087%)) 
testMintOnPayIfOneTierIsPassed(uint16) (gas: -7572 (-0.088%)) 
testMintOnPayIfOneTierIsPassed(uint16) (gas: -7572 (-0.088%)) 
testMintOnPayIfOneTierIsPassed(uint16) (gas: -7572 (-0.088%)) 
testMintOnPayUsingFallbackTiers(uint16) (gas: -7572 (-0.090%)) 
testMintOnPayUsingFallbackTiers(uint16) (gas: -7572 (-0.090%)) 
testMintOnPayUsingFallbackTiers(uint16) (gas: -7572 (-0.090%)) 
testDeployAndLaunchProject() (gas: -7572 (-0.093%)) 
testDeployAndLaunchProject() (gas: -7572 (-0.093%)) 
testDeployAndLaunchProject() (gas: -7572 (-0.093%)) 
testLaunchProjectFor_shouldLaunchProject(uint128) (gas: -7572 (-0.153%)) 
testLaunchProjectFor_shouldLaunchProject_nonFuzzed() (gas: -7572 (-0.153%)) 
Overall gas change: -223386 (-2.696%)
```

The range for all changes is approximately 5000-8000gas, with some large outliers, probably due to loops. 

### Remove revert check on `JB721TieredGovernance.getPastTierVotes()`

[Location](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L107)

#### Description

The `JB721TieredGovernance` contract contains functions for retrieving data from storage using the `Checkpoints` import. 

The function `Checkpoints.getAtBlock()` already contains a `require` test to make sure the block being requested isn't the current block or later. While the `if revert` syntax used in `JB721TieredGovernance` is cheaper than the `require`, in the success case the same condition is tested twice, wasting gas. 

Although this is a `view` function, it's plausible that it would be consumed by external integrations to make decisions based on voting power.

#### Mitigation

Remove the extraneous check.

#### Extra

If the check is required/intended for some other purpose, then `getPastTierVotes()` in the same contract should be updated to perform the same check, as it uses the same underlying call.

### Rewrite function to not duplicate computation.

Locations:
[1](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L105)
[2](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L258-L263)

#### Description

`JB721Delegate.didRedeem()` and `JB721Delegate.redeemParams()` contain some byte slicing to access a value that is treated as a `bytes4`. However, that same value could be extracted via a call to `abi.decode` already present later in the function.

Even with the `uint256[]` allocation from `abi.decode` before all of the revert checks have completed, it is still cheaper to move the line, name the `bytes4` value and use it in the boolean check.

```
(bytes4 _interfaceId, uint256[] memory _decodedTokenIds) = abi.decode(_data.metadata, (bytes4, uint256[]));

// Check the 4 bytes interfaceId and handle the case where the metadata was not intended for this contract
if ( _data.metadata.length < 4  
    || _interfaceId != type(IJB721Delegate).interfaceId) { 
    revert INVALID_REDEMPTION_METADATA();
}

// Get a reference to the number of token IDs being checked.
uint256 _numberOfTokenIds = _decodedTokenIds.length;

```

#### Savings

Implementing the change in both applicable locations saves ~13200 gas throughout the test suite, with outliers at ~170 gas. A sample of `forge snapshot --diff` is provided:

```
testJBTieredNFTRewardDelegate_didPay_mintFirstBestTierIfMultipleAvailableAtSameFloor() (gas: -13235 (-0.169%)) 
testJBTieredNFTRewardDelegate_getvotingUnits_returnsTheTotalVotingUnits() (gas: -13236 (-0.170%)) 
testJBTieredNFTRewardDelegate_balanceOf_returnsCompleteBalance_coverage() (gas: -13236 (-0.170%)) 
testJBTieredNFTRewardDelegate_tiers_returnsAllTiersExcludingRemovedOnes_coverage() (gas: -13237 (-0.170%)) 
testJBTieredNFTRewardDelegate_adjustTiers_removeTiers(uint16,uint256,uint8) (gas: -12935 (-0.170%)) 
testJBTieredNFTRewardDelegate_tiers_returnsAllTiers_coverage() (gas: -13236 (-0.170%)) 
testJBTieredNFTRewardDelegate_cleanTiers_removeTheInactiveTiers(uint16,uint256,uint8) (gas: -13114 (-0.172%)) 
testJBTieredNFTRewardDelegate_totalRedemptionWeight_returnsCorrectTotalWeightAsFloorsCumSum_coverage() (gas: -13236 (-0.172%)) 
testJBTieredNFTRewardDelegate_totalSupply_returnsTotalSupply_coverage() (gas: -13236 (-0.172%)) 
testJBTieredNFTRewardDelegate_tier_returnsTheGivenTier_coverage() (gas: -39686 (-0.172%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithReservedRate_coverage() (gas: -13236 (-0.172%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithVotingPower_coverage() (gas: -13236 (-0.172%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfEmptyQuantity_coverage() (gas: -13236 (-0.172%)) 
testJBTieredNFTRewardDelegate_redemptionWeightOf_returnsCorrectWeightAsFloorsCumSum_coverage() (gas: -13236 (-0.172%)) 
testJBTieredNFTRewardDelegate_mintReservesFor_revertIfReservedMintingIsPausedInFundingCycle() (gas: -13235 (-0.173%)) 
testJBTieredNFTRewardDelegate_tiers_returnsAllTiers(uint16) (gas: -13235 (-0.175%)) 
testJBTieredNFTRewardDelegate_tier_returnsTheGivenTier(uint16,uint16) (gas: -13235 (-0.175%)) 
testJBTieredNFTRewardDelegate_mintFor_revertIfManualMintNotAllowed() (gas: -13235 (-0.175%)) 
testJBTieredNFTRewardDelegate_getvotingUnits_returnsTheTotalVotingUnits(uint16,address) (gas: -13235 (-0.175%)) 
testJBTieredNFTRewardDelegate_balanceOf_returnsCompleteBalance(uint16,address) (gas: -13236 (-0.175%)) 
testJBTieredNFTRewardDelegate_totalRedemptionWeight_returnsCorrectTotalWeightAsFloorsCumSum(uint16) (gas: -13236 (-0.175%)) 
testJBTieredNFTRewardDelegate_totalSupply_returnsTotalSupply(uint16) (gas: -13236 (-0.175%)) 
testJBTieredNFTRewardDelegate_redemptionWeightOf_returnsCorrectWeightAsFloorsCumSum(uint16,uint16,uint16) (gas: -13235 (-0.176%)) 
testLaunchProjectFor_shouldLaunchProject(uint128) (gas: -13233 (-0.269%)) 
testLaunchProjectFor_shouldLaunchProject_nonFuzzed() (gas: -13233 (-0.269%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithVotingPower(uint8,uint8) (gas: -71272 (-0.889%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithReservedRate(uint8,uint8) (gas: -71347 (-0.890%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfEmptyQuantity(uint8,uint8) (gas: -76315 (-0.953%)) 
Overall gas change: -1412710 (-15.835%)
```

### Modify `if` statement with different cast for gas saving

Locations:
[1](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L236-L237)
[2](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L528)

#### Description

Some functions contain a check to only perform an action if an argument isn't the zero address.

The test can be modified slightly to be symantically identical and save gas by avoiding a more-expensive cast.

```
// Set the token URI resolver if provided.
//if (_tokenUriResolver != IJBTokenUriResolver(address(0))) { 
if (address(_tokenUriResolver) != address(0)) {
    _store.recordSetTokenUriResolver(_tokenUriResolver);
}
```

#### Savings

This method saves 3200-3800 gas.


```
Running 1 test for contracts/forge-test/governance/JB721GlobalGovernance.t.sol:TestJBGlobalGovernance
[PASS] testMintAndTransferGlobalVotingUnits(uint8,bool) (runs: 256, μ: 8964598, ~: 8933856)
Test result: ok. 1 passed; 0 failed; finished in 1.80s
testMintAndTransferGlobalVotingUnits(uint8,bool) (gas: -3827 (-0.043%)) 
Overall gas change: -3827 (-0.043%)
```