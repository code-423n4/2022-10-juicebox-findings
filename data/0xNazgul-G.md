## [NAZ-G1] For array elements, `arr[i] = arr[i] + 1` is cheaper than `arr[i] += 1`
**Context**: [`JBTiered721DelegateStore.sol#L827`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L827)

**Description**:
Due to stack operations this is 25 gas cheaper when dealing with arrays in storage, and 4 gas cheaper for memory arrays.

**Recommendation**: 
Use `arr[i] = arr[i] + 1` instead of `arr[i] += 1` when dealing with arrays.
