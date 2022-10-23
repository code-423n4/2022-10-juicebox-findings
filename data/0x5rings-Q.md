Findings: inconsistent solidity pragma versioning. 

Description: Ensure the contract versioning is consistent. Some contracts such as 'JBTiered721DelegateDeployer.sol' show
pragma solidity ^0.8.16. Whereas, JBDeployTiered721DelegateData.sol shows pragma solidity ^0.8.0;


Source: https://github.com/jbx-protocol/juice-nft-rewards/blob/7d7aec6f8642c6e1c5e55cfed67eb69b6fc8174a/contracts/structs/JB721Tier.sol#L2-L3

