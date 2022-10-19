Please lock all solidity compiler versions for best optimization

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L312
delete *_tokenId* as it has never needed

One suggestion is to use custom error Revert to process exception in most write transactions

