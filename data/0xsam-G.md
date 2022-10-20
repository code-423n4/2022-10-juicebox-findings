# Gas Optimization

## ++i costs less gas than i++ and i+=1 (--i/i--/i-=1 too).

    File: contracts/libraries/JBIpfsDecoder.sol

        digitlength++;

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L59

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _length; i++) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _input.length; i++) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _indices.length; i++) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84


## Initializing a variable to its default value costs unnecessary gas.

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _source.length; ++i) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49

    File: contracts/libraries/JBIpfsDecoder.sol

      for (uint256 j = 0; j < digitlength; ++j) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L51

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _length; i++) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _input.length; i++) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _indices.length; i++) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84


## Array length should not be looked up in every loop. Instead, use a variable to store the length before the loop starts.

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _source.length; ++i) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _input.length; i++) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76

    File: contracts/libraries/JBIpfsDecoder.sol

    for (uint256 i = 0; i < _indices.length; i++) {

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84

