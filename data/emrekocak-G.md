## Use assembly to check for address(0)  
Saves 6 gas per instance if using assembly to check for address(0)  
e.g.  
  
```  
assembly {  
 if iszero(_addr) {  
  mstore(0x00, "zero address")  
  revert(0x00, 0x20)  
 }  
}  
```  
  
Instances include:
[`contracts/JBTiered721DelegateDeployer.sol:99`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L99)
[`contracts/JB721TieredGovernance.sol:282`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L282)
[`contracts/JB721TieredGovernance.sol:291`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L291)
[`contracts/JBTiered721Delegate.sol:105`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L105)
[`contracts/JBTiered721Delegate.sol:146`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L146)
[`contracts/JBTiered721DelegateStore.sol:609`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L609)
[`contracts/JBTiered721DelegateStore.sol:872`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L872)
[`contracts/JBTiered721DelegateStore.sol:877`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L877)

## Use calldata instead of memory for function parameters  
  
It is generally cheaper to load variables directly from calldata, rather than copying them to memory. Only use memory if the variable needs to be modified.  
  
Instances include: 
[`contracts/JBTiered721DelegateStore.sol:628`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L628)
[`contracts/abstract/JB721Delegate.sol:311`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L311)
[`contracts/abstract/JB721Delegate.sol:323`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L323)
[`contracts/libraries/JBIpfsDecoder.sol:44`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L44)
[`contracts/libraries/JBIpfsDecoder.sol:66`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L66)
[`contracts/libraries/JBIpfsDecoder.sol:74`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L74)
[`contracts/libraries/JBIpfsDecoder.sol:82`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L82)
