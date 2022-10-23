# Gas Report
## [G-01] Using `calldata` instead of `memory` for read-only arguments in `external` functions saves gas

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * `<mem_array>.length`). Using `calldata` directly, obliviates the need for such a loop in the contract code and runtime execution.

If the array is passed to an `internal` function which passes the array to another `internal` function where the array is modified and therefore memory is used in the `external` call, it's still more gass-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one

_There are 7 instance(s) of this issue:_

[JBTiered721Delegate#mintReservesFor](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L264)

```
File: contracts/JBTiered721Delegate.sol    #1

264:  function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)
```

[JBTiered721Delegate#mintFor](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290)

```
File: contracts/JBTiered721Delegate.sol    #2

290:  function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)
```

[JBTiered721Delegate#setBaseUri](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L386)

```
File: contracts/JBTiered721Delegate.sol    #3

386:  function setBaseUri(string memory _baseUri) external override onlyOwner { 
```

[JBTiered721DelegateProjectDeployer#launchProjectFor](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L72)

```
File: contracts/JBTiered721DelegateProjectDeployer.sol    #4

72:        JBDeployTiered721DelegateData memory _deployTiered721DelegateData,
```

[JBTiered721DelegateProjectDeployer#launchFundingCyclesFor](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L109)

```
File: contracts/JBTiered721DelegateProjectDeployer.sol    #5

109:        JBDeployTiered721DelegateData memory _deployTiered721DelegateData,
```

[JBTiered721DelegateProjectDeployer#reconfigureFundingCyclesOf](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L152)

```
File: contracts/JBTiered721DelegateProjectDeployer.sol    #6

152:        JBDeployTiered721DelegateData memory _deployTiered721DelegateData,
```

[JBTiered721DelegateStore#recordBurn](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1091)

```
File: contracts/JBTiered721DelegateStore.sol    #7

1091:        function recordBurn(uint256[] memory _tokenIds) external override {  
```

## [G-02] Using `bool`s for storage incurs overhead

Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess ([100 gas](https://gist.github.com/IllIllI000/1b70014db712f8572a72378321250058)), and to avoid Gsset (20000 gas) when changing from 'false' to 'true', after having been 'true' in the past

_There are 3 instance(s) of this issue:_

[JBTiered721Delegate](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L543)

```
File: contracts/JBTiered721Delegate.sol    #1

543:   bool _expectMintFromExtraFunds;
```

[JBTiered721Delegate](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L546)

```
File: contracts/JBTiered721Delegate.sol    #2

546:   bool _dontOverspend; 
```

[JBTiered721Delegate](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L555)

```
File: contracts/JBTiered721Delegate.sol    #3

555:   bool _dontMint;
```