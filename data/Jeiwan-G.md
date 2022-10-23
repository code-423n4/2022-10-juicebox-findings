# Optimize NFT delegate deployments by using proxy
## Targets
- https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L115
## Impact
The cost of NFT delegate deployments can be significantly reduced by deploying proxies instead of clones of the implementation.
## Proof of Concept
This function is used to deploy new NFT delegates ([JBTiered721DelegateDeployer.sol#L115](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L115)):
```solidity
function _clone(address _targetAddress) internal returns (address _out) {
  assembly {
    // Get deployed/runtime code size
    let _codeSize := extcodesize(_targetAddress)

    // Get a bit of freemem to land the bytecode, not updated as we'll leave this scope right after create(..)
    let _freeMem := mload(0x40)

    // Shift the length to the length placeholder, in the constructor
    let _mask := mul(_codeSize, 0x100000000000000000000000000000000000000000000000000000000)

    // Insert the length in the correct sport (after the PUSH3 / 0x62)
    let _initCode := or(_mask, 0x62000000600081600d8239f3fe00000000000000000000000000000000000000)

    // Store the deployment bytecode
    mstore(_freeMem, _initCode)

    // Copy the bytecode (our initialise part is 13 bytes long)
    extcodecopy(_targetAddress, add(_freeMem, 13), 0, _codeSize)

    // Deploy the copied bytecode
    _out := create(0, _freeMem, _codeSize)
  }
```
It copies the code of an existing contract (`JBTiered721Delegate`, `JB721TieredGovernance`, or `JB721GlobalGovernance`) and deploys a new contract with the same code. This is a costly operation because each of the three contracts is a big contract with a lot of code. It'll be much cheaper to deploy non-upgradable proxies instead.
## Tools Used
Manual review.
## Recommended Mitigation Steps
Consider using [the Clones library from OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/proxy/Clones.sol)–it deploys and absolutely minimal non-upgradable proxy contract. Such proxies, however, [cannot be verified on Etherscan](https://forum.openzeppelin.com/t/how-to-verify-contracts-created-using-clonesupgradeable-clonedeterministic/7746/3). [Some more info](https://blog.openzeppelin.com/workshop-recap-cheap-contract-deployment-through-clones/).