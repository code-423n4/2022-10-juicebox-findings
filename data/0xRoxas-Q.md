# QA Report
## NC Found [1]

### [NC-01] Require Statements Missing Error Message

Error messages help with troubleshooting, some require statements are missing error messages.

#### Findings:

*/contracts/JBTiered721Delegate.sol*
Line(s): [216](https://github.com/jbx-protocol/juice-nft-rewards/blob/main/contracts/JBTiered721Delegate.sol#L216), [218](https://github.com/jbx-protocol/juice-nft-rewards/blob/main/contracts/JBTiered721Delegate.sol#L218)
```solidity
216:	require(address(this) != codeOrigin);
218:	require(address(store) == address(0));
```