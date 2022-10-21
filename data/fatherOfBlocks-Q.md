**JBTiered721DelegateDeployer**
- L79/81 - An attempt is made to evaluate which value within the JB721GovernanceType enum has the _deployTiered721DelegateData.governanceType parameter, but the enum only has 3 values, but a final else is generated that reverts after validating the three possible values.
It could also be implemented with a switch and 3 options and if the final revert is not necessary the INVALID_GOVERNANCE_TYPE() error would be unnecessary.


**JBTiered721Delegate**
- L36 - A PRICING_RESOLVER_CHANGES_PAUSED error is created, but it is never used in the entire contract.

- L216/218 - When we use a require and throw an exception it is important to show a message, this is important because it makes the user better understand the reason why it is reverted. 


**libraries/JBTiered721FundingCycleMetadataResolver**
- L4 - The JBTiered721FundingCycleMetadata struct is imported, but it is never used, so it should be removed.


**libraries/JBTiered721DelegateStore**
- L4 - The entire JBConstants library is imported just for a constant, this seems too unnecessary to me, a less expensive solution would be to create the same constant in the contract.

- L37 - A PRICING_RESOLVER_CHANGES_LOCKED error is created, but it is never used in the entire contract.

