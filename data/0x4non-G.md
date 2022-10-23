
##  x += y costs more gas than x = x + y for state variables (save 5% according to test)
Using the addition operator instead of plus-equals saves 113 gas.

```diff
diff --git a/contracts/JBTiered721DelegateStore.sol b/contracts/JBTiered721DelegateStore.sol
index df836c9..fc27444 100644
--- a/contracts/JBTiered721DelegateStore.sol
+++ b/contracts/JBTiered721DelegateStore.sol
@@ -351,7 +351,7 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
       _storedTier = _storedTierOf[_nft][_i];
 
       // Increment the total supply with the amount used already.
-      supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;
+      supply = supply + _storedTier.initialQuantity - _storedTier.remainingQuantity;
 
       unchecked {
         --_i;
@@ -406,7 +406,7 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
 
       if (_balance != 0)
         // Add the tier's voting units.
-        units += _balance * _storedTierOf[_nft][_i].votingUnits;
+        units = units + _balance * _storedTierOf[_nft][_i].votingUnits;
 
       unchecked {
         --_i;
@@ -503,7 +503,7 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
     // Loop through all tiers.
     for (uint256 _i = _maxTierId; _i != 0; ) {
       // Get a reference to the account's balance in this tier.
-      balance += tierBalanceOf[_nft][_owner][_i];
+      balance = balance + tierBalanceOf[_nft][_owner][_i];
 
       unchecked {
         --_i;
@@ -531,7 +531,7 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
 
     // Add each token's tier's contribution floor to the weight.
     for (uint256 _i; _i < _numberOfTokenIds; ) {
-      weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
+      weight = weight + _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
 
       unchecked {
         ++_i;
@@ -560,7 +560,7 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
       _storedTier = _storedTierOf[_nft][_i + 1];
 
       // Add the tier's contribution floor multiplied by the quantity minted.
-      weight +=
+      weight = weight +
         (_storedTier.contributionFloor *
           (_storedTier.initialQuantity - _storedTier.remainingQuantity)) +
         _numberOfReservedTokensOutstandingFor(_nft, _i, _storedTier);
```

### Tests gas snapshot
```
testJBTieredNFTRewardDelegate_didPay_revertIfTierRemoved() (gas: -400 (-0.345%)) 
testJBTieredNFTRewardDelegate_didPay_doesNotRevertOnAmountBelowContributionFloorIfNoMetadata() (gas: -400 (-0.385%)) 
testJBTieredNFTRewardDelegate_didPay_revertIfAmountTooLow() (gas: -400 (-0.414%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithVotingPower(uint8,uint8) (gas: -41046 (-0.505%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithReservedRate(uint8,uint8) (gas: -41163 (-0.506%)) 
testJBTieredNFTRewardDelegate_didPay_doesNotMintIfMetadataDeactivateMint() (gas: -400 (-0.520%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfEmptyQuantity(uint8,uint8) (gas: -42392 (-0.521%)) 
testJBTieredNFTRewardDelegate_didPay_revertIfAllowanceRunsOutInParticularTier() (gas: -40400 (-0.625%)) 
testJBTieredNFTRewardDelegate_didPay_mintBestTierIfNonePassed(uint8) (gas: 3375 (1.793%)) 
Overall gas change: -257120 (-5.114%)
```


## Use unchecked on safe increments to save up to 15.549% of gas according to test;

```diff
diff --git a/contracts/JBTiered721DelegateStore.sol b/contracts/JBTiered721DelegateStore.sol
index df836c9..11c0401 100644
--- a/contracts/JBTiered721DelegateStore.sol
+++ b/contracts/JBTiered721DelegateStore.sol
@@ -557,7 +557,7 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
     // Add each token's tier's contribution floor to the weight.
     for (uint256 _i; _i < _maxTierId; ) {
       // Keep a reference to the stored tier.
-      _storedTier = _storedTierOf[_nft][_i + 1];
+      unchecked { _storedTier = _storedTierOf[_nft][_i + 1]; } // impossible to overflow `_i + 1`
 
       // Add the tier's contribution floor multiplied by the quantity minted.
       weight +=
@@ -682,7 +682,8 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
       if (_tierToAdd.initialQuantity == 0) revert NO_QUANTITY();
 
       // Get a reference to the tier ID.
-      uint256 _tierId = _currentMaxTierIdOf + _i + 1;
+      uint256 _tierId;
+      unchecked { _tierId = _currentMaxTierIdOf + _i + 1; } // impossible to overflow `_i + 1`
 
       // Add the tier with the iterative ID.
       _storedTierOf[msg.sender][_tierId] = JBStored721Tier({
@@ -732,9 +733,11 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
             _tierToAdd.contributionFloor <
             _storedTierOf[msg.sender][_currentSortIndex].contributionFloor
           ) {
-            // If the index being iterated on isn't the next index, set the after.
-            if (_currentSortIndex != _tierId + 1)
-              _tierIdAfter[msg.sender][_tierId] = _currentSortIndex;
+            unchecked {              
+              // If the index being iterated on isn't the next index, set the after.
+              if (_currentSortIndex != _tierId + 1)
+                _tierIdAfter[msg.sender][_tierId] = _currentSortIndex;
+            } // impossible to overflow `_tierId + 1`
 
             // If this is the last tier being added, track the current last sort index if it's not already tracked.
             if (
@@ -758,8 +761,10 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
           }
           // If the tier being iterated on is the last tier, add the tier after it.
           else if (_next == 0 || _next > _currentMaxTierIdOf) {
-            if (_tierId != _currentSortIndex + 1)
-              _tierIdAfter[msg.sender][_currentSortIndex] = _tierId;
+            unchecked {
+              if (_tierId != _currentSortIndex + 1)
+                _tierIdAfter[msg.sender][_currentSortIndex] = _tierId;
+            } // impossible to overflow `_currentSortIndex + 1`
 
             // For the next tier being added, start at this current index.
             _startSortIndex = _tierId;
@@ -1191,11 +1196,13 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
 
       if (!_bitmapWord.isTierIdRemoved(_currentSortIndex)) {
         // If the current index being iterated on isn't an increment of the previous, set the correct tier after if needed.
-        if (_currentSortIndex != _previous + 1) {
-          if (_tierIdAfter[_nft][_previous] != _currentSortIndex)
-            _tierIdAfter[_nft][_previous] = _currentSortIndex;
-          // Otherwise if the current index is an increment of the previous and the after index isn't 0, set it to 0.
-        } else if (_tierIdAfter[_nft][_previous] != 0) _tierIdAfter[_nft][_previous] = 0;
+        unchecked {
+          if (_currentSortIndex != _previous + 1) {
+            if (_tierIdAfter[_nft][_previous] != _currentSortIndex)
+              _tierIdAfter[_nft][_previous] = _currentSortIndex;
+            // Otherwise if the current index is an increment of the previous and the after index isn't 0, set it to 0.
+          } else if (_tierIdAfter[_nft][_previous] != 0) _tierIdAfter[_nft][_previous] = 0;
+        } // impossible to overflow `_precious + 1`
 
         // Set the previous index to be the current index.
         _previous = _currentSortIndex;
@@ -1300,7 +1307,7 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
     uint256 _storedNext = _tierIdAfter[_nft][_index];
     if (_storedNext != 0) return _storedNext;
     // Otherwise increment the current.
-    return _index + 1;
+    unchecked { return _index + 1; } // impossible to overflow `_i + 1`
   }
 
   /** 
```

### Tests gas snapshot
```
.......
testJBTieredNFTRewardDelegate_adjustTiers_addAndRemoveTiers() (gas: -13327 (-0.308%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfRemovingALockedTier_coverage() (gas: -11797 (-0.318%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfRemovingALockedTier(uint16,uint8) (gas: -11377 (-0.321%)) 
testJBTieredNFTRewardDelegate_constructor_deployIfNoEmptyInitialQuantity_coverage() (gas: -12097 (-0.322%)) 
testJBTieredNFTRewardDelegate_constructor_deployIfNoEmptyInitialQuantity(uint16) (gas: -10957 (-0.324%)) 
testJBTieredNFTRewardDelegate_adjustTiers_addNewTiers_coverage() (gas: -13075 (-0.327%)) 
testJBTieredNFTRewardDelegate_constructor_revertDeploymentIfOneEmptyInitialQuantity_coverage() (gas: -11227 (-0.328%)) 
testJBTieredNFTRewardDelegate_constructor_revertDeploymentIfOneEmptyInitialQuantity(uint16,uint16) (gas: -10957 (-0.335%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithVotingPower(uint8,uint8) (gas: -60073 (-0.738%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithReservedRate(uint8,uint8) (gas: -60181 (-0.740%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfEmptyQuantity(uint8,uint8) (gas: -61316 (-0.754%)) 
testJBTieredNFTRewardDelegate_didPay_mintBestTierIfNonePassed(uint8) (gas: 3400 (1.806%)) 
Overall gas change: -1385918 (-15.549%)
```


## Cache mappings on loops to save 9.51% of gas according to test;

```diff
diff --git a/contracts/JBTiered721DelegateStore.sol b/contracts/JBTiered721DelegateStore.sol
index df836c9..36f0e59 100644
--- a/contracts/JBTiered721DelegateStore.sol
+++ b/contracts/JBTiered721DelegateStore.sol
@@ -230,6 +230,8 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
     // Keep a referecen to the tier being iterated on.
     JBStored721Tier memory _storedTier;
 
+    mapping(uint256 => uint256) storage _isTierRemo = _isTierRemoved[_nft];
+
     // Initialise a BitmapWord for isRemoved
     JBBitmapWord memory _bitmapWord = _isTierRemoved[_nft].readId(_currentSortIndex);
 
@@ -237,7 +239,7 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
     while (_currentSortIndex != 0 && _numberOfIncludedTiers < _size) {
       // Is the current index outside the currently stored word for isRemoved?
       if (_bitmapWord.refreshBitmapNeeded(_currentSortIndex))
-        _bitmapWord = _isTierRemoved[_nft].readId(_currentSortIndex);
+        _bitmapWord = _isTierRemo.readId(_currentSortIndex);
 
       if (!_bitmapWord.isTierIdRemoved(_currentSortIndex)) {
         _storedTier = _storedTierOf[_nft][_currentSortIndex];
@@ -399,14 +401,17 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
     // Keep a reference to the balance being iterated on.
     uint256 _balance;
 
+    mapping(uint256 => uint256) storage tierBalanceOfNftAccount =  tierBalanceOf[_nft][_account];
+    mapping(uint256 => JBStored721Tier) storage _storedTierOfNft = _storedTierOf[_nft];
+
     // Loop through all tiers.
     for (uint256 _i = _maxTierId; _i != 0; ) {
       // Get a reference to the account's balance in this tier.
-      _balance = tierBalanceOf[_nft][_account][_i];
+      _balance = tierBalanceOfNftAccount[_i];
 
       if (_balance != 0)
         // Add the tier's voting units.
-        units += _balance * _storedTierOf[_nft][_i].votingUnits;
+        units += _balance * _storedTierOfNft[_i].votingUnits;
 
       unchecked {
         --_i;
@@ -529,9 +534,11 @@ contract JBTiered721DelegateStore is IJBTiered721DelegateStore {
     // Get a reference to the total number of tokens.
     uint256 _numberOfTokenIds = _tokenIds.length;
 
+    mapping(uint256 => JBStored721Tier) storage _storedTierOfNft = _storedTierOf[_nft];
+
     // Add each token's tier's contribution floor to the weight.
     for (uint256 _i; _i < _numberOfTokenIds; ) {
-      weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
+      weight += _storedTierOfNft[tierIdOfToken(_tokenIds[_i])].contributionFloor;
 
       unchecked {
         ++_i;
```


### Tests gas snapshot
```
testJBTieredNFTRewardDelegate_adjustTiers_addAndRemoveTiers() (gas: -6806 (-0.157%)) 
testJBTieredNFTRewardDelegate_adjustTiers_addNewTiers(uint16,uint16[]) (gas: -6806 (-0.161%)) 
testJBTieredNFTRewardDelegate_adjustTiers_addNewTiers_coverage() (gas: -6806 (-0.170%)) 
testJBTieredNFTRewardDelegate_constructor_deployIfNoEmptyInitialQuantity_coverage() (gas: -6801 (-0.181%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfRemovingALockedTier_coverage() (gas: -6806 (-0.183%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfRemovingALockedTier(uint16,uint8) (gas: -6806 (-0.192%)) 
testJBTieredNFTRewardDelegate_constructor_revertDeploymentIfOneEmptyInitialQuantity_coverage() (gas: -6811 (-0.199%)) 
testJBTieredNFTRewardDelegate_constructor_deployIfNoEmptyInitialQuantity(uint16) (gas: -6801 (-0.201%)) 
testJBTieredNFTRewardDelegate_constructor_revertDeploymentIfOneEmptyInitialQuantity(uint16,uint16) (gas: -6811 (-0.208%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithVotingPower(uint8,uint8) (gas: -60050 (-0.738%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithReservedRate(uint8,uint8) (gas: -60470 (-0.743%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfEmptyQuantity(uint8,uint8) (gas: -64880 (-0.798%)) 
testJBTieredNFTRewardDelegate_didPay_mintBestTierIfNonePassed(uint8) (gas: 1887 (1.003%)) 
Overall gas change: -855175 (-9.518%)
```


## Update Openzeppellin contract to latest version to save around 1%
Change vesion from 4.6.0 to 4.7.3
` @openzeppelin/contracts  ^4.6.0  →  ^4.7.3`

```
testRedeemAll() (gas: 0 (0.000%)) 
testRedeemToken(uint16) (gas: 0 (0.000%)) 
testMintAndTransferGlobalVotingUnits(uint8,bool) (gas: 1914 (0.021%)) 
testMintAndTransferTieredVotingUnits(uint8,bool) (gas: 3827 (0.042%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithVotingPower(uint8,uint8) (gas: -27362 (-0.336%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfAddingWithReservedRate(uint8,uint8) (gas: -27593 (-0.339%)) 
testJBTieredNFTRewardDelegate_adjustTiers_revertIfEmptyQuantity(uint8,uint8) (gas: -30019 (-0.369%)) 
Overall gas change: -79233 (-0.981%)
```