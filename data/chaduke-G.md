https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L133
as *>=* is more expensive than *<*
Change the code to the following:

    if (_blockNumber < block.number) return _totalTierCheckpoints[_tier].getAtBlock(_blockNumber);
    else revert BLOCK_NOT_YET_MINED();

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L147
change the argument from memory data to calldata to save gas

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L160
No need to cache it into _data, user the *_setTierDelegatesData* calldata variable directly. 

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L313
change *_tier* to a calldata argument to save gas

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol
change codeOrigin, store, fundingCycleStore, prices, pricingCurrency, pricingDecimals, creditsOf to immmutable or possiblly private too since they will set once by the constructor to save gas

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L233
check !=0 is cheaper than checking 
change 240 to :  if (_pricing.tiers.length != 0) _store.recordAddTiers(_pricing.tiers);

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L202
First of all, change *memory _mintReservesForTiersData* to *calldata _mintReservesForTiersData* to save gas since the argument will never be updated. Second, no need to cache it to *_data* in 273, read each field from *_mintReservesForTiersData* directly to save gas.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290
First of all, change *memory _mintForTiersData* to *calldata _mintForTiersData* to save gas since the argument will never be updated. Second, no need to cache it to *_data* in 300, read each field from *_mintForTiersData* directly to save gas.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L52
since it is a storage variable, it can save gas by changing
    self[_depth] |= uint256(1 << (_index % 256));
to 
    self[_depth] = self[_depth] | uint256(1 << (_index % 256));    
if the function is called within another write transaction

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L44
if function *_toBase58* will be called in another write transaction, then there would be several gas-saving opportunities:
- *_source* can be changed to a calldata variable as it has never been updated
- _source.length should be cached in a local variable to avoid repeated call in a for loop
-     for (uint256 i = 0; i < _source.length; ++i) should be changed to
          for (uint256; i < sourcelength;  unchecked{++i}) to avoid unnecessary initialization, repleated calling of _source.length (use the cached variable), and the unnecessary check
-  change 
            for (uint256 j = 0; j < digitlength; ++j)
   to 
                  for (uint256 j; j < digitlength;  unchecked{++j}) for similar reason

- Line 59, change digitallength++ to ++digitallength to save gas

similar for loop optimization can be done for lines 68, 76, and 84.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L66
_array can be changed to a calldata variable as it is never updated in the function

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L74
_input can be changed to a calldata variable as it is never updated in the function

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L83
_indices can be changed to a calldata variable as it is never updated in the function
 