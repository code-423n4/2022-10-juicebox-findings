# QA ISSUES  FOR JUICE-NFT-REWARDS

##  [N-01]  Adding a `return` statement when the function defines a named return variable, is redundant


./contracts/JBTiered721Delegate.sol
```
L613:  function _mintBestAvailableTier(
L631:  return leftoverAmount;
```

##  [L-02]  Not emiting events in some important functions
Description: Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

./contracts/JBTiered721Delegate.sol
```
L202:  function initialize(
```

./contracts/abstract/JB721Delegate.sol
```
L203:  function _initialize(
```

##  [N-03]  Non-library/interface files should use fixed compiler versions, not floating ones


./contracts/JB721TieredGovernance.sol
```
L2:    pragma solidity ^0.8.16;
```

./contracts/JBTiered721DelegateProjectDeployer.sol
```
L2:    pragma solidity ^0.8.16;
```

./contracts/JB721GlobalGovernance.sol
```
L2:    pragma solidity ^0.8.16;
```

./contracts/JBTiered721DelegateDeployer.sol
```
L2:    pragma solidity ^0.8.16;
```

./contracts/JBTiered721DelegateStore.sol
```
L2:    pragma solidity ^0.8.16;
```

./contracts/abstract/JB721Delegate.sol
```
L2:    pragma solidity ^0.8.16;
```

./contracts/JBTiered721Delegate.sol
```
L2:    pragma solidity ^0.8.16;
```

##  [N-04]  Not using the NatSpec format

./contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol
```
L1:    // SPDX-License-Identifier: MIT
```