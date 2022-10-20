- [Low](#low)
    - [**1. Outdated packages**](#1-outdated-packages)
    - [**2. Use string.concat instead of abi.encodePacked**](#2-use-stringconcat-instead-of-abiencodepacked)
    - [**3. Integer overflow by unsafe casting**](#3-integer-overflow-by-unsafe-casting)
    - [**4. Lack of checks supportsInterface**](#4-lack-of-checks-supportsinterface)
    - [**5. Lack of initialize protections**](#5-lack-of-initialize-protections)

# Low

## **1. Outdated packages**

Some used packages are out of date, it is good practice to use the latest version of these packages:

```
"@openzeppelin/contracts": "^4.6.0"
```

**Reference:**

- https://github.com/OpenZeppelin/openzeppelin-contracts/releases

## **2. Use `string.concat` instead of `abi.encodePacked`**

Tthere is a discussion about removing `abi.encodePacked` from future versions of Solidity ([ethereum/solidity#11593](https://github.com/ethereum/solidity/issues/11593)), so using `abi.encode` now will ensure compatibility in the future.

**Affected source code:**

- [JBIpfsDecoder.sol:28](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L28)
- [JBIpfsDecoder.sol:34](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L34)

## **3. Integer overflow by unsafe casting**

Keep in mind that the version of solidity used, despite being greater than `0.8`, does not prevent integer overflows during casting, it only does so in mathematical operations.

It is necessary to safely convert between the different numeric types.

**Recommendation:**

Use a [safeCast](https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast) from Open Zeppelin.

```javascript
  function tierIdOfToken(uint256 _tokenId) public pure override returns (uint256) {
    // The tier ID is in the first 16 bits.
    return uint256(uint16(_tokenId));
  }
```

**Affected source code:**

- [JBTiered721DelegateStore.sol:587](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L587)
- [JBTiered721DelegateStore.sol:689](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L689)
- [JBTiered721DelegateStore.sol:690](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L690)
- [JBTiered721DelegateStore.sol:691](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L691)
- [JBTiered721DelegateStore.sol:692](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L692)
- [JBTiered721DelegateStore.sol:693](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L693)
- [JBTiered721DelegateStore.sol:694](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L694)
- [JBIpfsDecoder.sol:50](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L50)

## **4. Lack of checks `supportsInterface`**

The `EIP-165` standard helps detect that a smart contract implements the expected logic, prevents human error when configuring smart contract bindings, so it is recommended to check that the received argument is a contract and supports the expected interface.

**Reference:**

- https://eips.ethereum.org/EIPS/eip-165

**Affected source code:**

- [JB721Delegate.sol:212](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L212)
- [JBTiered721DelegateDeployer.sol:51-53](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L51-L53)
- [JBTiered721DelegateProjectDeployer.sol:52-53](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L52-L53)

## **5. Lack of `initialize` protections**

The following contracts are updateable, and follow the update pattern, these contracts contain an `initialize` method to set the contract once, but anyone can call this method, this will spend project fuel if someone tries to initialize it before the project.

It is recommended to check the sender when initializing the contract.

**Affected source code:**

- [JBTiered721Delegate.sol:202-214](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L202-L214)


