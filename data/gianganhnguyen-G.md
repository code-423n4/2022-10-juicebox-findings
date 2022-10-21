# 1. [G-1] For loops: ++i  cost less gas compare to i++

Instances include:

    File contracts/libraries/JBIpfsDecoder.sol, line 68:       for (uint256 i = 0; i < _length; i++) {
    File contracts/libraries/JBIpfsDecoder.sol, line 76:       for (uint256 i = 0; i < _input.length; i++) {
    File contracts/libraries/JBIpfsDecoder.sol, line 84:       for (uint256 i = 0; i < _indices.length; i++) {

I suggest using `++i` instead of `i++` to increment the value of an uint variable.

# 2. [G-2] For loops: Cache the length of arrays in the loops to save gas

    File contracts/libraries/JBIpfsDecoder.sol, line 49:       for (uint256 i = 0; i < _source.length; ++i) {
    File contracts/libraries/JBIpfsDecoder.sol, line 76:       for (uint256 i = 0; i < _input.length; i++) {
    File contracts/libraries/JBIpfsDecoder.sol, line 84:       for (uint256 i = 0; i < _indices.length; i++) {

I suggest storing the array’s length in a variable before the for-loop, and use it instead.

# 3. [G-3] For loops: increments in for loop can be uncheck to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block

    File contracts/libraries/JBIpfsDecoder.sol, line 49:       for (uint256 i = 0; i < _source.length; ++i) {
    File contracts/libraries/JBIpfsDecoder.sol, line 51:       for (uint256 j = 0; j < digitlength; ++j) {
    File contracts/libraries/JBIpfsDecoder.sol, line 68:       for (uint256 i = 0; i < _length; i++) {
    File contracts/libraries/JBIpfsDecoder.sol, line 76:       for (uint256 i = 0; i < _input.length; i++) {
    File contracts/libraries/JBIpfsDecoder.sol, line 84:       for (uint256 i = 0; i < _indices.length; i++) {

The code would go from:

    for (uint256 i; i < numIterations; i++) { 
      ...
    }

to:

    for (uint256 i; i < numIterations;) { 
      ...
      unchecked { ++i; }  
    }

# 4. [G-4] Variables: No need to explicitly initialize variables with default values

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

Instances include:

    File contracts/libraries/JBIpfsDecoder.sol, line 49:       for (uint256 i = 0; i < _source.length; ++i) {
    File contracts/libraries/JBIpfsDecoder.sol, line 51:       for (uint256 j = 0; j < digitlength; ++j) {
    File contracts/libraries/JBIpfsDecoder.sol, line 68:       for (uint256 i = 0; i < _length; i++) {
    File contracts/libraries/JBIpfsDecoder.sol, line 76:       for (uint256 i = 0; i < _input.length; i++) {
    File contracts/libraries/JBIpfsDecoder.sol, line 84:       for (uint256 i = 0; i < _indices.length; i++) {

I suggest removing explicit initializations for default values.

# 5. [G-5] Variable: Incrementing and Decrementing by 1, ++number cost less gas compare number++ or number += 1

I suggest using `++number` instead of `number++` or `number += 1` here:

    File contracts/JBTiered721DelegateStore.sol, line 1106:      numberOfBurnedFor[msg.sender][_tierId]++;
    File contracts/JBTiered721DelegateStore.sol, line 1108:      _storedTierOf[msg.sender][_tierId].remainingQuantity++;

    File contracts/libraries/JBIpfsDecoder.sol, line 59:       digitlength++;

# 6. [G-6] Parameter: If we are not modifying the passed parameter we should pass it as `calldata` because `calldata` is more gas efficient than `memory`.

I suggest using `calldata` instead of `memory` here:

    File contracts/JB721GlobalGovernance.sol, function `_afterTokenTransferAccounting` - line 51.

    File contracts/JB721TieredGovernance.sol, functions: `setTierDelegates` - line 147, `_afterTokenTransferAccounting` - line 313.

    File contracts/JBTiered721Delegate.sol, functions: `mintReservesFor` - line 264,  `mintFor` - line 290, `mintFor` - line 480, 

    File contracts/JBTiered721DelegateDeployer.sol, functions: `deployDelegateFor` - line 69.  

    File contracts/JBTiered721DelegateStore.sol, functions: `recordAddTiers` - line 628,  `recordBurn` - line 1091, `_numberOfReservedTokensOutstandingFor` - line 1224.

    File contracts/libraries/JBIpfsDecoder.sol, functions: `_toBase58` - line 44, `_truncate` - line 66, `_reverse` - line 74, `_toAlphabet` - line 82.
