1. It is good practice to use error strings in `require` statements

Using error strings in require statements enables the caller to be notified as to why the call failed if it fails. Consider adding understandable strings in `require` statements as error strings.

Instances: 
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L216
https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L218 