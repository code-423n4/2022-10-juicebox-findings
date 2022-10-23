### G-01 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

*Number of Instances Identified: 5*

Removing unused named returns variables can reduce gas usage (MSTOREs/MLOADs) and improve code clarity. To save gas and improve code quality: consider using only one of those.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol

```
37: returns (uint256 units)
123: function balanceOf(address _owner) public view override returns (uint256 balance) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
617: ) internal returns (uint256 leftoverAmount) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol

```
119: returns (uint256 configuration)
162: returns (uint256 configuration)
```


### G-02 X += Y COSTS MORE GAS THAN X = X + Y FOR STATE VARIABLES

*Number of Instances Identified: 7*

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

```
354: supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;
409: units += _balance * _storedTierOf[_nft][_i].votingUnits;
506: balance += tierBalanceOf[_nft][_owner][_i];
534: weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
563: weight +=...
827: numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
62: carry += uint256(digits[j]) * 256;
```


### G-03 ++I or I++ SHOULD BE UNCHECKED{++I} or UNCHECKED{I++} WHEN IT IS NOT POSSIBLE FOR THEM TO OVERFLOW, AS IS THE CASE WHEN USED IN FOR- AND WHILE-LOOPS

the `unchecked` keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. 

*Number of Instances Identified: 5*

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
49: for (uint256 i = 0; i < _source.length; ++i) {
51: for (uint256 j = 0; j < digitlength; ++j) {
68: for (uint256 i = 0; i < _length; i++) {
76: for (uint256 i = 0; i < _input.length; i++) {
84: for (uint256 i = 0; i < _indices.length; i++) {
```

### G-04 ++I COSTS LESS GAS COMPARED TO I++ OR I += 1 (SAME FOR --I VS I-- OR I -= 1)

*Number of Instances Identified: 6*

Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

-   `i += 1` is the most expensive form
-   `i++` costs 6 gas less than `i += 1`
-   `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

-   `i -= 1` is the most expensive form
-   `i--` costs 11 gas less than `i -= 1`
-   `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
59: digitlength++;
68: for (uint256 i = 0; i < _length; i++) {
76: for (uint256 i = 0; i < _input.length; i++) {
84: for (uint256 i = 0; i < _indices.length; i++) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

```
1106: numberOfBurnedFor[msg.sender][_tierId]++;
1108: _storedTierOf[msg.sender][_tierId].remainingQuantity++;
```


### G-05 IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED

*Number of Instances Identified: 5*

If a variable is not set/initialized, it is assumed to have the default value (`0` for `uint`, `false` for `bool`, `address(0)` for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

As an example: `for (uint256 i = 0; i < numIterations; ++i) {` should be replaced with `for (uint256 i; i < numIterations; ++i) {`

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
49: for (uint256 i = 0; i < _source.length; ++i) {
51: for (uint256 j = 0; j < digitlength; ++j) {
68: for (uint256 i = 0; i < _length; i++) {
76: for (uint256 i = 0; i < _input.length; i++) {
84: for (uint256 i = 0; i < _indices.length; i++) {
```

### G-06 INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

*Number of Instances Identified: 4*

Not inlining costs **20 to 40 gas** because of two extra `JUMP` instructions and additional stack operations needed for function calls.

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol

```
194: function _getTierVotingUnits(address _account, uint256 _tierId)
242: function _transferTierVotingUnits
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
524: function _processPayment(JBDidPayData calldata _data) internal override {  
598: function _didBurn(uint256[] memory _tokenIds) internal override {
```
