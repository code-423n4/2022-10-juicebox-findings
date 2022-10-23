# Lack of state variable projectId caching in JB721Delegate functions
State variable ```projectId``` is accessed twice in functions ```didPay``` and ```didRedeem```.

Add ```uint256 _projectId = projectId;``` at the start of both functions, and replace every occurency of ```projectId``` in this functions for the new variable.

# Lack of state variable pricingCurrency caching in JBTiered721Delegate function
```pricingCurrency``` can be put in a memory variable in function ```_processPayment``` to avoid double access to state variable.

# Lack of state variable store caching in JBTiered721Delegate functions
```store``` can be put in a memory variable to avoid double access to state variable. In functions
* JBTiered721Delegate#mintReservesFor: Lines [448](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L448) and [451](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L451)
* [JBTiered721Delegate#_beforeTokenTransfer](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L723-L749): Lines [731](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L731), [744](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L744) and [745](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L745)
* [JBTiered721Delegate#_afterTokenTransfer](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L765-L768)

