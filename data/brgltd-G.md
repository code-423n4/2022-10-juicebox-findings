# [01] The increment in for loop post condition can be made unchecked to save gas

It can save 30-40 gas [per loop](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked)

There are 5 instances of this issue.

```
File: contracts/libraries/JBIpfsDecoder.sol
49: for (uint256 i = 0; i < _source.length; ++i) {
51: for (uint256 j = 0; j < digitlength; ++j) {
68: for (uint256 i = 0; i < _length; i++) {
76: for (uint256 i = 0; i < _input.length; i++) {
84: for (uint256 i = 0; i < _indices.length; i++) {
```

# [02] x += y costs more gas than x = x + y for state variables

Using the addition operator instead of plus-equals saves 113 [gas](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8).

There's 1 instance with this issue.

```
File: contracts/JBTiered721DelegateStore.sol
827: numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

# [03] Cache state variable reads

Caching of a state variable replace each Gwarmaccess (100 gas) with a cheaper stack read.

For example, on the following instance, `projectId` should be cached.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L232-L233
