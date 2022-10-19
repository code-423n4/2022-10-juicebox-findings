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

    