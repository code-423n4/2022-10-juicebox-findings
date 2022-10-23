# Issues

## 1\. _clone() in JBTiered721DelegateDeplyer.sol trims last 13 bytes of deployed contract code

The code in _clone() inserts a tiny initialization code before the deployed bytecode. It uses the deployed bytecode size in the create() call, which expects initialization bytecode size. This means 13 bytes, the initialization bytecode size, are trimmed from the source contract when copied to the target contract. These bytes contain compiler metadata as discussed [here](https://docs.sourcify.dev/blog/verify-contracts-perfectly/.) . The consequence is that contract verifiers like etherscan will fail to verify the source code of project delegates.