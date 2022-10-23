Please lock all solidity compiler versions for best optimization

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L312
delete *_tokenId* as it has never needed

One suggestion is to use custom error Revert to process exception in most write transactions

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L177

Use a locked version of solidity compiler for all contracts


https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L177
Suggest to change function *setTierDelegate* to external if it will not be called by the contract itself

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L44-L53
Consider combing the two mappings into one mapping from address->uint256->struct

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L216-L218
use custom error revert instead of *require* without an error msg

I suggest to lock all solidity compiler versions for all files and use the most recent version

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol
all the three functions can be simplified to save gas if these functions are called in other write transactions. 

change
        return (_data & 1) == 1;
to 
       return _data & 1;

change 
       return ((_data >> 1) & 1) == 1;
to 
       return (_data & 2) == 2;

change
         return ((_data >> 2) & 1) == 1;
to 
       return (_data  & 4) == 4;

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1123
Consider to combine several of mappings Address - X (.....maxTierIdOf, tierBalanceOf, numberOfReservesMintedFor, numberOfBurnedFor, defaultReservedTokenBeneficiaryOf, firstOwnerOf, baseUriOf, tokenUriResolverOf, contractUriOf, encodedIPFSUriOf)  into one mapping Address-> Struct to improve code readability and possibly gas saving due to packing
In particular define 
struct NFTContractInfo {
    mapping(uint256 => uint256) maxTierIdOf;
    mapping(uint256 => address) _reservedTokenBeneficiaryOf;
    mapping(uint256 => JBStored721Tier) _storedTierOf;
    JBTiered721Flags  _flagsOf;
    mapping(uint256 => uint256) _isTierRemoved;
    uint256 trackedLastSortTierIdOf;
    uint256 maxTierIdOf;
    mapping(address => mapping(uint256 => uint256))  tierBalanceOf;
    ....
}
Then define a mapping(address => NFTContractInfo)
This is just an example, the authors can define several structs as well, to group related variables in one struct, for example. 

 


