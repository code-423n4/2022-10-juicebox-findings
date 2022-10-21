1.Unused Custom error defined which is cause directly a waste of gas:

error PRICING_RESOLVER_CHANGES_LOCKED();
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L37

error PRICING_RESOLVER_CHANGES_PAUSED();
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L36
 
2.!= 0 is a cheaper operation compared to > 0, when dealing with uint.

if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1254

    if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L240

