# Report

## Non-Critical Issues ##

### [N-01]: Function defines a named return variable but then it uses return statements
**Context:** 
 
1. ``` return store.votingUnitsOf(address(this), _account); ``` [L39](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L39)
2. ``` return _launchFundingCyclesFor(_projectId, _launchFundingCyclesData); ``` [L134](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L134)
3. ``` return _reconfigureFundingCyclesOf(_projectId, _reconfigureFundingCyclesData); ``` [L177](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L177)
4. ``` return store.balanceOf(address(this), _owner); ``` [L124](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L124)

**Recommendation:**

Choose named return variable or return statement. It is unnecessary to use both.

### [N-02]: Wrong order of functions
**Context:** 

1. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L185 (constructor must be before all functions)
2. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol (all external functions must be before all public functions)
3. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol (all external functions must be before all public functions)
4. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L203 (internal functions must be after all external and public functions)
5. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L228, https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L249 (all external functions must be before all public and all internal functions)

**Description:**

According [official solidity documentation](https://docs.soliditylang.org/en/v0.8.6/style-guide.html#order-of-functions) functions should be grouped according to their visibility and ordered:

+ constructor

+ receive function (if exists)

+ fallback function (if exists)

+ external

+ public

+ internal

+ private

**Recommendation:**

Put the functions in the correct order according to the documentation.


### [N-03]: NatSpec is missing

1. [JBTiered721FundingCycleMetadataResolver.sol](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol)
2. @param tag and @return tag are missing in all 6 functions [JBBitmap.sol](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol)
3. NatSpec is missing in 4 functions [JBIpfsDecoder.sol](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol)


### [N-04]: Public function can be external
**Context:** 

1. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L147
2. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L177
3. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L123
4. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L138
5. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L202
6. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L523
7. https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L550

**Description:**

Public functions can be declared external if they are not called by the contract.

**Recommendation:**

Declare these functions as external instead of public.


### [N-05]: Typos
**Context:** 

1. ``` @param _tokenId The id of the token for which voting units are being transfered. ``` [L48](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L48) (change ***transfered*** to ***transferred***) 
2. ``` JBOperatable: Several functions in this contract can only be accessed by a project owner, or an address that has been preconfifigured to be an operator of the project. ``` [L19](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L19) (change ***preconfifigured*** to ***preconfigured***) 
3. ``` The contract responsibile for deploying the delegate. ``` [L34](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L34) (change ***responsibile*** to ***responsible***) 
4. ``` @param _blockNumber the blocknumber to check the voting power at ``` [L100](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L100) (change ***blocknumber*** to ***block number***) 
5. ``` @param _tierId The ID of the tier for which voting units are being transfered. ``` [L209](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L209) (change ***transfered*** to ***transferred***) 
6. ``` @param _tierId The ID of the tier for which voting units are being transfered. ``` [L239](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L239) (change ***transfered*** to ***transferred***)
7. ``` @param _tierId The ID of the tier for which voting units are being transfered. ``` [L269](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L269) (change ***transfered*** to ***transferred***)
8. ``` @return balance The number of tokens owners by the owner accross all tiers. ``` [L121](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L121) (change ***accross*** to ***across***)  
9. ``` @param _interfaceId The ID of the interface to check for adherance to. ``` [L173](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L173) (change ***adherance*** to ***adherence***) 
10. ``` Sets the beneificiary of the reserved tokens for tiers where a specific beneficiary isn't set. ``` [L363](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L363) (change ***beneificiary*** to ***beneficiary***) 
11. ``` @param _beneficiary The default beneificiary of the reserved tokens. ``` [L368](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L368) (change ***beneificiary*** to ***beneficiary***) 
12. ``` // Keep a reference to the flag indicating if the transaction should revert if all provded funds aren't spent. ``` [L545](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L545) (change ***provded*** to ***provided***) 
13. ``` User the hook to register the first owner if it's not yet regitered. ``` [L717](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L717) (change ***regitered*** to ***registered***)  
14. ``` @param _tokenId The ID of the token being transfered. ``` [L721](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L721) (change ***transfered*** to ***transferred***) 
15. ``` // Transfered must not be paused when not minting or burning. ``` [L728](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L728) (change ***Transfered*** to ***Transferred***) 
16. ``` @param _tokenId The ID of the token being transfered. ``` [L757](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L757) (change ***transfered*** to ***transferred***) 
17. ``` @param _tokenId The ID of the token for which voting units are being transfered. ``` [L782](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L782) (change ***transfered*** to ***transferred***) 
18. ``` // Keep a referecen to the tier being iterated on. ``` [L230](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L230) (change ***referecen*** to ***reference***)  
19. ``` @return balance The number of tokens owners by the owner accross all tiers. ``` [L497](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L497) (change ***accross*** to ***across***) 
20. ``` @return The reserved token benficiary. ``` [L597](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L597) (change ***benficiary*** to ***beneficiary***) 
21. ``` // Keep a reference to the idex to iterate on next. ``` [L719](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L719) (change ***idex*** to ***index***) 
22. ``` @param _beneficiary The reservd token beneficiary. ``` [L852](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L852) (change ***reservd*** to ***reserved***) 
23. ``` @param _tierId The ID the tier being transfered ``` [L862](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L862) (change ***transfered*** to ***transferred***) 
24. ``` // Forward the recieved weight and memo, and use this contract as a pay delegate. ``` [L88](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L88) (change ***recieved*** to ***received***) 
25. ``` @param _interfaceId The ID of the interface to check for adherance to. ``` [L176](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L176) (change ***adherance*** to ***adherence***)