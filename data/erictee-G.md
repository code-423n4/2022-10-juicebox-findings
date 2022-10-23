### [G-01] Cache Array Length Outside of Loop


#### Impact
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.


#### Findings:
```

contracts/libraries/JBIpfsDecoder.sol:L49    for (uint256 i = 0; i < _source.length; ++i) {

contracts/libraries/JBIpfsDecoder.sol:L76    for (uint256 i = 0; i < _input.length; i++) {

contracts/libraries/JBIpfsDecoder.sol:L84    for (uint256 i = 0; i < _indices.length; i++) {

```

### [G-02] Functions guaranteed to revert when called by normal users can be declared as payable.


#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.


#### Findings:
```
contracts/JBTiered721Delegate.sol:L370  function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {

contracts/JBTiered721Delegate.sol:L386  function setBaseUri(string memory _baseUri) external override onlyOwner {

contracts/JBTiered721Delegate.sol:L402  function setContractUri(string calldata _contractUri) external override onlyOwner {

contracts/JBTiered721Delegate.sol:L418  function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {

```

### [G-03] ++i costs less gas than i++, especially when it's used in for loops.


#### Impact
Saves 6 gas per loop.


#### Findings:
```

contracts/libraries/JBIpfsDecoder.sol:L59        digitlength++;

contracts/libraries/JBIpfsDecoder.sol:L68    for (uint256 i = 0; i < _length; i++) {

contracts/libraries/JBIpfsDecoder.sol:L76    for (uint256 i = 0; i < _input.length; i++) {

contracts/libraries/JBIpfsDecoder.sol:L84    for (uint256 i = 0; i < _indices.length; i++) {

```

### [G-04] Explicit initialization with zero not required


#### Impact
Explicit initialization with zero is not required for variable declaration because uints are 0 by default. Removing this will reduce contract size and save a bit of gas.


#### Findings:
```
contracts/libraries/JBIpfsDecoder.sol:L49    for (uint256 i = 0; i < _source.length; ++i) {

contracts/libraries/JBIpfsDecoder.sol:L51      for (uint256 j = 0; j < digitlength; ++j) {

contracts/libraries/JBIpfsDecoder.sol:L68    for (uint256 i = 0; i < _length; i++) {

contracts/libraries/JBIpfsDecoder.sol:L76    for (uint256 i = 0; i < _input.length; i++) {

contracts/libraries/JBIpfsDecoder.sol:L84    for (uint256 i = 0; i < _indices.length; i++) {

```

### [G-05] ```++i```/```i++``` should be ```unchecked{++i}```/```unchecked{i++}``` when it is not possible for them to overflow, as is the case when used in for- and while-loops.


#### Impact
The ```unchecked``` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop.


#### Findings:
```
contracts/libraries/JBIpfsDecoder.sol:L49    for (uint256 i = 0; i < _source.length; ++i) {

contracts/libraries/JBIpfsDecoder.sol:L68    for (uint256 i = 0; i < _length; i++) {

contracts/libraries/JBIpfsDecoder.sol:L76    for (uint256 i = 0; i < _input.length; i++) {

contracts/libraries/JBIpfsDecoder.sol:L84    for (uint256 i = 0; i < _indices.length; i++) {

```
