## [G-1] Pre-increment (++i) costs less gas than Post-increment (i++), especially in for loops.

Pre-increment saves 5 gas per loop.

***File : JBIpfsDecoder.sol***

JBIpfsDecoder.sol#L68
JBIpfsDecoder.sol#L76
JBIpfsDecoder.sol#L84




## [G-2] It costs more gas to initialize variables to zero than to let the default of zero be applied.

if a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

***File : JBIpfsDecoder.sol***

JBIpfsDecoder.sol#L49
JBIpfsDecoder.sol#L51
JBIpfsDecoder.sol#L68
JBIpfsDecoder.sol#L76
JBIpfsDecoder.sol#L84




## [G-3] ++i/i++ should be UNCHECKED{++i}/UNCHECKED{i++} when it is not possible for them to overflow (eg. in for and while loops).

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves around 40 gas per loop.

***File : JBIpfsDecoder.sol***

JBIpfsDecoder.sol#L49
JBIpfsDecoder.sol#L68
JBIpfsDecoder.sol#L76
JBIpfsDecoder.sol#L84




## [G-4] Non-strict inequalities are cheaper than strict ones.

Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas). This also holds true between <= and <.

Multiple instances accros the different contacts.



## [G-5] Functions guaranteed to revert when called by normal users (eg. onlyOwner) can be marked payable.

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

***File : JBTiered721Delegate.sol***

JBTiered721Delegate.sol#L418
JBTiered721Delegate.sol#L386
JBTiered721Delegate.sol#L402
JBTiered721Delegate.sol#L370




## [G-6] multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

***File : JBTiered721DelegateStore.sol***

JBTiered721DelegateStore.sol#L57
JBTiered721DelegateStore.sol#L66
JBTiered721DelegateStore.sol#L75
JBTiered721DelegateStore.sol#L83
JBTiered721DelegateStore.sol#L93
JBTiered721DelegateStore.sol#L104
JBTiered721DelegateStore.sol#L119
JBTiered721DelegateStore.sol#L129
JBTiered721DelegateStore.sol#L138
JBTiered721DelegateStore.sol#L147
JBTiered721DelegateStore.sol#L155
JBTiered721DelegateStore.sol#L164
JBTiered721DelegateStore.sol#L172
JBTiered721DelegateStore.sol#L180
JBTiered721DelegateStore.sol#L188
JBTiered721DelegateStore.sol#L197

***File : JB721TieredGovernance.sol***

JB721TieredGovernance.sol#L44
JB721TieredGovernance.sol#L53
