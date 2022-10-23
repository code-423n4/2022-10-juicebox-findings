# Juicebox Contest QA Report

## Summary

The following QA issues were found during the code audit:

1. Unspecific Compiler Version Pragma (10 instances)
2. `block.number` may exceed `type(uint32).max` (1 instance)
3. Named return variables are not used (5 instances)

Total 16 instances of 3 issues.

## 1. Unspecific Compiler Version Pragma (10 instances)

Avoid floating pragmas for non-library contracts. While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain. It is recommended to pin to a concrete compiler version.

```solidity
contracts/JB721GlobalGovernance.sol::2 => pragma solidity ^0.8.16;

contracts/JBTiered721DelegateDeployer.sol::2 => pragma solidity ^0.8.16;

contracts/JBTiered721DelegateProjectDeployer.sol::2 => pragma solidity ^0.8.16;

contracts/JB721TieredGovernance.sol::2 => pragma solidity ^0.8.16;

contracts/JBTiered721Delegate.sol::2 => pragma solidity ^0.8.16;

contracts/JBTiered721DelegateStore.sol::2 => pragma solidity ^0.8.16;

contracts/abstract/JB721Delegate.sol::2 => pragma solidity ^0.8.16;

contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol::2 => pragma solidity ^0.8.16;

contracts/libraries/JBBitmap.sol::2 => pragma solidity ^0.8.16;

contracts/libraries/JBIpfsDecoder.sol::2 => pragma solidity ^0.8.16;
```

## 2. `block.number` may exceed `type(uint32).max` (1 instance)

While this currently equates to around 1260 years, if there's a hard fork which makes block times much more frequent (e.g. to compete with Solana), then this limit may be reached much faster than expected, and transfers and delegations will remain stuck at their existing settings.

```solidity
contracts/JB721TieredGovernance.sol::133 => if (_blockNumber >= block.number) revert BLOCK_NOT_YET_MINED();
```

## 3. Named return variables are not used (5 instances)

Consider changing the variable to be an unnamed one, or use the named returns variables instead of `return` statements.

```solidity
File: contracts/JB721GlobalGovernance.sol::32-40

  function _getVotingUnits(address _account)
    internal
    view
    virtual
    override
    returns (uint256 units)
  {
    return store.votingUnitsOf(address(this), _account);
  }
```

```solidity
File: contracts/JBTiered721Delegate.sol::123-125

  function balanceOf(address _owner) public view override returns (uint256 balance) {
    return store.balanceOf(address(this), _owner);
  }
```

```solidity
File: contracts/JBTiered721Delegate.sol::613-638

  function _mintBestAvailableTier(
    uint256 _amount,
    address _beneficiary,
    bool _expectMint
  ) internal returns (uint256 leftoverAmount) {
    // Keep a reference to the token ID.
    uint256 _tokenId;

    // Keep a reference to the tier ID.
    uint256 _tierId;

    // Record the mint.
    (_tokenId, _tierId, leftoverAmount) = store.recordMintBestAvailableTier(_amount);

    // If there's no best tier, return or revert.
    if (_tokenId == 0) {
      // Make sure a mint was not expected.
      if (_expectMint) revert NOT_AVAILABLE();
      return leftoverAmount;
    }

    // Mint the tokens.
    _mint(_beneficiary, _tokenId);

    emit Mint(_tokenId, _tierId, _beneficiary, _amount - leftoverAmount, msg.sender);
  }
```

```solidity
File: contracts/JBTiered721DelegateProjectDeployer.sol::107-135

  function launchFundingCyclesFor(
    uint256 _projectId,
    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,
    JBLaunchFundingCyclesData memory _launchFundingCyclesData
  )
    external
    override
    requirePermission(
      controller.projects().ownerOf(_projectId),
      _projectId,
      JBOperations.RECONFIGURE
    )
    returns (uint256 configuration)
  {
    // Deploy the delegate contract.
    IJBTiered721Delegate _delegate = delegateDeployer.deployDelegateFor(
      _projectId,
      _deployTiered721DelegateData
    );

    // Set the delegate address as the data source of the provided metadata.
    _launchFundingCyclesData.metadata.dataSource = address(_delegate);

    // Set the project to use the data source for its pay function.
    _launchFundingCyclesData.metadata.useDataSourceForPay = true;

    // Launch the funding cycles.
    return _launchFundingCyclesFor(_projectId, _launchFundingCyclesData);
  }
```

```solidity
File: contracts/JBTiered721DelegateProjectDeployer.sol::150-178

  function reconfigureFundingCyclesOf(
    uint256 _projectId,
    JBDeployTiered721DelegateData memory _deployTiered721DelegateData,
    JBReconfigureFundingCyclesData memory _reconfigureFundingCyclesData
  )
    external
    override
    requirePermission(
      controller.projects().ownerOf(_projectId),
      _projectId,
      JBOperations.RECONFIGURE
    )
    returns (uint256 configuration)
  {
    // Deploy the delegate contract.
    IJBTiered721Delegate _delegate = delegateDeployer.deployDelegateFor(
      _projectId,
      _deployTiered721DelegateData
    );

    // Set the delegate address as the data source of the provided metadata.
    _reconfigureFundingCyclesData.metadata.dataSource = address(_delegate);

    // Set the project to use the data source for its pay function.
    _reconfigureFundingCyclesData.metadata.useDataSourceForPay = true;

    // Reconfigure the funding cycles.
    return _reconfigureFundingCyclesOf(_projectId, _reconfigureFundingCyclesData);
  }
```
