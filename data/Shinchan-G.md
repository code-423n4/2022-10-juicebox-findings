# **Gas Optimization**

Serial No. | Issue Name | Instances
--- | --- | ---
G-1 | Strict inequalities (>) are more expensive than non-strict ones (>=) | 2
G-2 | x += y costs more gas than x = x + y for state variables | 7
G-3 | Use calldata instead of memory in external functions | 3
G-4 | Using bools for storage incurs overhead | 3
G-5 | Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead | 7
G-6 | Use assembly to check for address(0) | 11

----
***Total instances found in this contest: 33 | Among all files in scope***

## G-01 Strict inequalities (>) are more expensive than non-strict ones (>=)

Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas. I suggest using >= instead of > to avoid some opcodes here:

### Instances:

[contracts/JBTiered721Delegate.sol:551:      _data.metadata.length > 36 &&](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L551)
[contracts/JBTiered721DelegateStore.sol:965:          _storedTier.contributionFloor > _bestContributionFloor &&](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L965)
### References:

[https://code4rena.com/reports/2022-04-badger-citadel/#g-31--is-cheaper-than](https://code4rena.com/reports/2022-04-badger-citadel/#g-31--is-cheaper-than)


-----

## G-02 x += y costs more gas than x = x + y for state variables

### Instances:
[contracts/JBTiered721DelegateStore.sol:354:      supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L354)
[contracts/JBTiered721DelegateStore.sol:409:        units += _balance * _storedTierOf[_nft][_i].votingUnits;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L409)
[contracts/JBTiered721DelegateStore.sol:506:      balance += tierBalanceOf[_nft][_owner][_i];](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L506)
[contracts/JBTiered721DelegateStore.sol:534:      weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L534)
[contracts/JBTiered721DelegateStore.sol:563:      weight +=](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L563)
[contracts/JBTiered721DelegateStore.sol:827:    numberOfReservesMintedFor[msg.sender][_tierId] += _count;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L827)
[contracts/libraries/JBIpfsDecoder.sol:52:        carry += uint256(digits[j]) * 256;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L52)
### References:

[https://github.com/code-423n4/2022-05-backd-findings/issues/108](https://github.com/code-423n4/2022-05-backd-findings/issues/108)


-----

## G-03 Use calldata instead of memory in external functions

Use calldata instead of memory in external functions to save gas.

### Instances:
[contracts/JBTiered721Delegate.sol:386:  function setBaseUri(string memory _baseUri) external override onlyOwner {](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L386)
[contracts/JBTiered721DelegateStore.sol:1016:  ) external override returns (uint256[] memory tokenIds, uint256 leftoverAmount) {](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1016)
[contracts/JBTiered721DelegateStore.sol:1091:  function recordBurn(uint256[] memory _tokenIds) external override {](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1091)
### Reference:

[https://code4rena.com/reports/2022-04-badger-citadel/#g-12-stakedcitadeldepositall-use-calldata-instead-of-memory](https://code4rena.com/reports/2022-04-badger-citadel/#g-12-stakedcitadeldepositall-use-calldata-instead-of-memory)


-----

## G-04 Using bools for storage incurs overhead

```
// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and
// pointer aliasing, and it cannot be disabled.
```

[Refer Here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

### Instances:
[contracts/JBTiered721Delegate.sol:543:    bool _expectMintFromExtraFunds;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L543)
[contracts/JBTiered721Delegate.sol:546:    bool _dontOverspend;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L546)
[contracts/JBTiered721Delegate.sol:555:      bool _dontMint;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L555)
### References:

[https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead](https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead)


-----

## G-05 Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed

### Instances:
[contracts/JBTiered721DelegateStore.sol:689:        contributionFloor: uint80(_tierToAdd.contributionFloor),](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L689)
[contracts/JBTiered721DelegateStore.sol:690:        lockedUntil: uint48(_tierToAdd.lockedUntil),](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L690)
[contracts/JBTiered721DelegateStore.sol:691:        remainingQuantity: uint40(_tierToAdd.initialQuantity),](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L691)
[contracts/JBTiered721DelegateStore.sol:692:        initialQuantity: uint40(_tierToAdd.initialQuantity),](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L692)
[contracts/JBTiered721DelegateStore.sol:693:        votingUnits: uint16(_tierToAdd.votingUnits),](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L693)
[contracts/JBTiered721DelegateStore.sol:694:        reservedRate: uint16(_tierToAdd.reservedRate),](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L694)
[contracts/libraries/JBIpfsDecoder.sol:48:    uint8 digitlength = 1;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L48)

-----


## G-06 Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, 'zero address')
  revert(0x00, 0x20)
 }
}
```
### Instances
[contracts/JBTiered721Delegate.sol:105:    if (_storedFirstOwner != address(0)) return _storedFirstOwner;](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L105)
[contracts/JBTiered721Delegate.sol:146:    if (address(_resolver) != address(0)) return _resolver.getUri(_tokenId);](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L146)
[contracts/JBTiered721Delegate.sol:729:    if (_from != address(0)) {](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L729)
[contracts/JBTiered721Delegate.sol:736:          _to != address(0) &&](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L736)
[contracts/JBTiered721DelegateStore.sol:609:    if (_storedReservedTokenBeneficiaryOfTier != address(0))](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L609)
[contracts/JBTiered721DelegateStore.sol:700:        _tierToAdd.reservedTokenBeneficiary != address(0) &&](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L700)
[contracts/JBTiered721DelegateStore.sol:872:    if (_from != address(0))](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L872)
[contracts/JBTiered721DelegateStore.sol:877:    if (_to != address(0)) {](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L877)
[contracts/JB721TieredGovernance.sol:282:    if (_from != address(0)) {](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L282)
[contracts/JB721TieredGovernance.sol:291:    if (_to != address(0)) {](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L291)
[contracts/JBTiered721DelegateDeployer.sol:99:    if (_deployTiered721DelegateData.owner != address(0))](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L99)


-----

