****************Out-of-scope***********************
////Low-Risk/////
In contract
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/structs/JB721Tier.sol#L24

  address reservedTokenBeneficiary;
but here in your comments it defined on the basis of 'Rate':   @member reservedRateBeneficiary The beneificary of the reserved tokens for this tier.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/structs/JB721Tier.sol#L12
***************************************************************


Issue:Low-Risk
1.Zero-address checks is missing in JBTiered721DelegateStore.tier and JBTiered721DelegateStore.balanceOf as it is considered a best-practise for input validation of critical address parameters.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L279 
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L499
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L523

Make a address check on parameter _nft,_owner:

2.Make a require check condition on JBTiered721DelegateStore.tierOfTokenId so that revert would not get execute on undesired input that can be made willingly/unwillingly will leads to waste of gas:

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L307

Make a check on _tokenId != 0: As here in this function it 'Return the tier for the specified token ID'. 