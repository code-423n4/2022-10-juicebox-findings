
#### Impact
Issue Information: [L001](https://github.com/byterocket/c4-common-issues/blob/main/2-Low-Risk.md#l001---unsafe-erc20-operations)

#### Findings:
```
contracts/forge-test/E2E.t.sol::153 => IERC721(NFTRewardDataSource).transferFrom(_beneficiary, address(696969420), tokenId);
contracts/forge-test/E2E.t.sol::159 => IERC721(NFTRewardDataSource).transferFrom(address(696969420), address(123456789), tokenId);
contracts/forge-test/E2E.t.sol::233 => IERC721(NFTRewardDataSource).transferFrom(_beneficiary, address(696969420), tokenId);
contracts/forge-test/NFTReward_Unit.t.sol::3971 => IERC721(delegate).transferFrom(msg.sender, beneficiary, _tokenId);
contracts/forge-test/NFTReward_Unit.t.sol::4088 => IERC721(_delegate).transferFrom(msg.sender, beneficiary, _tokenId);
contracts/forge-test/governance/JB721GlobalGovernance.t.sol::70 => ERC721(address(_delegate)).transferFrom(_user, _userFren, _generateTokenId(_tier + 1, 1));
contracts/forge-test/governance/JB721TieredGovernance.t.sol::73 => ERC721(address(_delegate)).transferFrom(_user, _userFren, _generateTokenId(_tier + 1, 1));
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

### Unspecific Compiler Version Pragma

#### Impact
Issue Information: [L003](https://github.com/byterocket/c4-common-issues/blob/main/2-Low-Risk.md#l003---unspecific-compiler-version-pragma)

#### Findings:
```
contracts/JB721GlobalGovernance.sol::2 => pragma solidity ^0.8.16;
contracts/JB721TieredGovernance.sol::2 => pragma solidity ^0.8.16;
contracts/JBTiered721Delegate.sol::2 => pragma solidity ^0.8.16;
contracts/JBTiered721DelegateDeployer.sol::2 => pragma solidity ^0.8.16;
contracts/JBTiered721DelegateProjectDeployer.sol::2 => pragma solidity ^0.8.16;
contracts/JBTiered721DelegateStore.sol::2 => pragma solidity ^0.8.16;
contracts/abstract/ERC721.sol::4 => pragma solidity ^0.8.16;
contracts/abstract/JB721Delegate.sol::2 => pragma solidity ^0.8.16;
contracts/abstract/Votes.sol::3 => pragma solidity ^0.8.0;
contracts/enums/JB721GovernanceType.sol::2 => pragma solidity ^0.8.0;
contracts/forge-test/Deployer_Unit.t.sol::1 => pragma solidity ^0.8.16;
contracts/forge-test/E2E.t.sol::1 => pragma solidity ^0.8.16;
contracts/forge-test/NFTReward_Unit.t.sol::1 => pragma solidity ^0.8.16;
contracts/forge-test/governance/JB721GlobalGovernance.t.sol::1 => pragma solidity ^0.8.16;
contracts/forge-test/governance/JB721TieredGovernance.t.sol::1 => pragma solidity ^0.8.16;
contracts/forge-test/utils/AccessJBLib.sol::2 => pragma solidity ^0.8.16;
contracts/forge-test/utils/TestBaseWorkflow.sol::2 => pragma solidity ^0.8.16;
contracts/interfaces/IJB721Delegate.sol::2 => pragma solidity ^0.8.0;
contracts/interfaces/IJB721TieredGovernance.sol::2 => pragma solidity ^0.8.0;
contracts/interfaces/IJBTiered721Delegate.sol::2 => pragma solidity ^0.8.0;
contracts/interfaces/IJBTiered721DelegateDeployer.sol::2 => pragma solidity ^0.8.0;
contracts/interfaces/IJBTiered721DelegateProjectDeployer.sol::2 => pragma solidity ^0.8.0;
contracts/interfaces/IJBTiered721DelegateStore.sol::2 => pragma solidity ^0.8.0;
contracts/libraries/JBBitmap.sol::2 => pragma solidity ^0.8.16;
contracts/libraries/JBIpfsDecoder.sol::2 => pragma solidity ^0.8.16;
contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol::2 => pragma solidity ^0.8.16;
contracts/scripts/Deploy.s.sol::1 => pragma solidity ^0.8.16;
contracts/scripts/LaunchProjectFor.s.sol::1 => pragma solidity ^0.8.16;
contracts/structs/JB721PricingParams.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JB721Tier.sol::2 => pragma solidity ^0.8.16;
contracts/structs/JB721TierParams.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBBitmapWord.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBDeployTiered721DelegateData.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBLaunchFundingCyclesData.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBLaunchProjectData.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBReconfigureFundingCyclesData.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBStored721Tier.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBTiered721Flags.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBTiered721FundingCycleMetadata.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBTiered721MintForTiersData.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBTiered721MintReservesForTiersData.sol::2 => pragma solidity ^0.8.0;
contracts/structs/JBTiered721SetTierDelegatesData.sol::2 => pragma solidity ^0.8.0;
```
#### Tools used
[c4udit](https://github.com/byterocket/c4udit)

