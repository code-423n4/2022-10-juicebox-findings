https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L133
as *>=* is more expensive than *<*
Change the code to the following:

    if (_blockNumber < block.number) return _totalTierCheckpoints[_tier].getAtBlock(_blockNumber);
    else revert BLOCK_NOT_YET_MINED();

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L147
change the argument from memory data to calldata to save gas

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L160
No need to cache it into _data, user the *_setTierDelegatesData* calldata variable directly. 


    