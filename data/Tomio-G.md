1.
Title: Default value initialization

Impact:
If a variable is not set/initialized, it is assumed to have the default value (0, false, 0x0 etc depending on the data type). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

Proof of Concept:
[JBIpfsDecoder.sol#L49](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49)
[JBIpfsDecoder.sol#L68](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68)
[JBIpfsDecoder.sol#L76](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)
[JBIpfsDecoder.sol#L84](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)

Recommended Mitigation Steps:
Remove explicit initialization for default values.
________________________________________________________________________

2.
Title: Using unchecked and prefix increment is more effective for gas saving:

Proof of Concept:
[JBIpfsDecoder.sol#L49](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49)
[JBIpfsDecoder.sol#L68](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68)
[JBIpfsDecoder.sol#L76](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)
[JBIpfsDecoder.sol#L84](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)

Recommended Mitigation Steps:
Change to:

```	
	for (uint256 i = 0; i < _indices.length; i++) {
	// ...
	unchecked { ++i; }
	}
```
________________________________________________________________________

3.
Title: Caching `length` for loop can save gas

Proof of Concept:
[JBIpfsDecoder.sol#L49](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49)
[JBIpfsDecoder.sol#L76](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)
[JBIpfsDecoder.sol#L84](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)

Recommended Mitigation Steps:
Change to:

```
	uint256 Length = _indices.length;
	for (uint256 i = 0; i < Length; i++) {
```
________________________________________________________________________