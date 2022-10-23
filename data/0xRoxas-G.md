# Gas Report
## Optimizations found [2]

**Summary Statement**
The main gas improvements found come from seperating conditions rather than using the binary operation &&. 

### [G-01] Use nested if instead of && [ⓘ](https://github.com/devanshbatham/Solidity-Gas-Optimization-Tips#6--use-nested-if-and-avoid-multiple-check-combinations)

#### Findings:

Using nested if statements saves [3 gas](https://yos.io/2021/05/17/gas-efficient-solidity/#tip-11-splitting-require-statements-that-use--saves-gas) per &&.

*/contracts/JBTiered721DelegateStore.sol*
Line(s): [664](https://github.com/jbx-protocol/juice-nft-rewards/blob/main/contracts/JBTiered721DelegateStore.sol#L664), [668](https://github.com/jbx-protocol/juice-nft-rewards/blob/main/contracts/JBTiered721DelegateStore.sol#L668), [678](https://github.com/jbx-protocol/juice-nft-rewards/blob/main/contracts/JBTiered721DelegateStore.sol#L678), [1050](https://github.com/jbx-protocol/juice-nft-rewards/blob/main/contracts/JBTiered721DelegateStore.sol#L1050)
```solidity
664:	if (_i != 0 && _tierToAdd.contributionFloor < _tiersToAdd[_i - 1].contributionFloor)
668:	if (_flags.lockVotingUnitChanges && _tierToAdd.votingUnits != 0)
678:	if (_flags.lockManualMintingChanges && _tierToAdd.allowManualMint)
1050:	if (_isManualMint && !_storedTier.allowManualMint) revert CANT_MINT_MANUALLY();
```

**Example:**
from
```solidity
if (_i != 0 && _tierToAdd.contributionFloor < _tiersToAdd[_i - 1].contributionFloor) {
	//Inner code
}
```
to 
```solidity
if (_i != 0) {
	if (_tierToAdd.contributionFloor < _tiersToAdd[_i - 1].contributionFloor) {
		//Inner code
	}
}
```

### [G-02] Unless Used for Variable Packing uint8 May Be More Expensive Than Using uint256 [ⓘ](https://yos.io/2021/05/17/gas-efficient-solidity/#tip-15-usage-of-uint8-may-increase-gas-cost)

The EVM operates 32 bytes at a time. Changing `uint8` to `uint256` unless used for variable packing will reduce gas costs.

#### Findings:

*/contracts/libraries/JBIpfsDecoder.sol*
Line(s): [48](https://github.com/jbx-protocol/juice-nft-rewards/blob/main/contracts/libraries/JBIpfsDecoder.sol#L48)
```solidity
48:	uint8 digitlength = 1;
```

**Note:** The parameter of `_trunctate` will also have to change