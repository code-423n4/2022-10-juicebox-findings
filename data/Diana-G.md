
## G-01 USAGE OF UINTS OR INTS SMALLER THAN 32 BYTES (26 BITS) INCURS OVERHEAD

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html) Use a larger size then downcast where needed

There are 11 instances of this issue:

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
File: contracts/JBTiered721Delegate.sol

480: function mintFor(uint16[] memory _tierIds, address _beneficiary)
558: uint16[] memory _tierIdsToMint;
563: (bytes32, bytes4, bool, bool, bool, uint16[])
652: uint16[] memory _mintTierIds,
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
File: contracts/libraries/JBIpfsDecoder.sol

46: uint8[] memory digits = new uint8[](46);
48: uint8 digitlength = 1;
66: function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
67: uint8[] memory output = new uint8[](_length);
74: function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {
75: uint8[] memory output = new uint8[](_input.length);
82: function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```

---------

## G-02 INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

Not inlining costs 20 to 40 gas because of two extra `JUMP` instructions and additional stack operations needed for function calls.

There are 4 instances of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol

```
File: contracts/JB721TieredGovernance.sol

194-199: function _getTierVotingUnits(address _account, uint256 _tierId)
    internal
    view
    virtual
    returns (uint256)
  {
242-247: function _transferTierVotingUnits(
    address _from,
    address _to,
    uint256 _tierId,
    uint256 _amount
  ) internal virtual {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
File: contracts/JBTiered721Delegate.sol

524: function _processPayment(JBDidPayData calldata _data) internal override {
598: function _didBurn(uint256[] memory _tokenIds) internal override {
```

----------

## G-03 x += y COSTS MORE GAS THAN x = x+y FOR STATE VARIABLES

There are 7 instances of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

```
File: contracts/JBTiered721DelegateStore.sol

354: supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;
409: units += _balance * _storedTierOf[_nft][_i].votingUnits;
506: balance += tierBalanceOf[_nft][_owner][_i];
534: weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
563-566: weight +=
        (_storedTier.contributionFloor *
          (_storedTier.initialQuantity - _storedTier.remainingQuantity)) +
        _numberOfReservedTokensOutstandingFor(_nft, _i, _storedTier);
827: numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L52

```
File: contracts/libraries/JBIpfsDecoder.sol

52: carry += uint256(digits[j]) * 256;
```
-----------

## G-04 IT COSTS MORE GAS TO INITIALIZE VARIABLES TO ZERO THAN TO LET THE DEFAULT OF ZERO BE APPLIED

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

There are 5 instances of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
File: contracts/libraries/JBIpfsDecoder.sol

49: for (uint256 i = 0; i < _source.length; ++i) {
51: for (uint256 j = 0; j < digitlength; ++j) {
68: for (uint256 i = 0; i < _length; i++) {
76: for (uint256 i = 0; i < _input.length; i++) {
84: for (uint256 i = 0; i < _indices.length; i++) {
```
---------------

## G-05 ++i COSTS LESS GAS THAN i++ ESPECIALLY WHEN IT'S USED IN FOR LOOPS (SAME APPLIES FOR --i, i-- TOO)

Saves 6 gas _PER LOOP_

Prefix increments are cheaper than postfix increments.  

Further more, using unchecked {++i} is even more gas efficient, and the gas saving accumulates every iteration and can make a real change  

But increments perform overflow checks that are not necessary in this case.  

These functions use not using prefix increments (`++i`) and/or not using the unchecked keyword:

There are 6 instances of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

```
File: contracts/JBTiered721DelegateStore.sol

1106: numberOfBurnedFor[msg.sender][_tierId]++;
1108: _storedTierOf[msg.sender][_tierId].remainingQuantity++;
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
File: contracts/libraries/JBIpfsDecoder.sol

59: digitlength++;
68: for (uint256 i = 0; i < _length; i++) {
76: for (uint256 i = 0; i < _input.length; i++) {
84: for (uint256 i = 0; i < _indices.length; i++) {
```
-------------

## G-06 INCREMENTS CAN BE UNCHECKED

In Solidity 0.8+, there’s a default overflow check on unsigned integers. It’s possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline

Prior to Solidity 0.8.0, arithmetic operations would always wrap in case of under- or overflow leading to widespread use of libraries that introduce additional checks.

Since Solidity 0.8.0, all arithmetic operations revert on over- and underflow by default, thus making the use of these libraries unnecessary.

To obtain the previous behaviour, an unchecked block can be used

There are 5 instances of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

```
File: contracts/libraries/JBIpfsDecoder.sol

49: for (uint256 i = 0; i < _source.length; ++i) {
51: for (uint256 j = 0; j < digitlength; ++j) {
68: for (uint256 i = 0; i < _length; i++) {
76: for (uint256 i = 0; i < _input.length; i++) {
84: for (uint256 i = 0; i < _indices.length; i++) {
```
----------------

## G-07 USING BOTH NAMED RETURNS AND A RETURN STATEMENT ISN'T NECESSARY

There are 4 instances of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L32-L40

```
File: contracts/JB721GlobalGovernance.sol

32-40: function _getVotingUnits(address _account)
    internal
    view
    virtual
    override
    returns (uint256 units)
  {
    return store.votingUnitsOf(address(this), _account);
  }
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L123-L125

```
File: contracts/JBTiered721Delegate.sol

123-125: function balanceOf(address _owner) public view override returns (uint256 balance) {
    return store.balanceOf(address(this), _owner);
  }
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol

```
File: contracts/JBTiered721DelegateProjectDeployer.sol

107-135:  function launchFundingCyclesFor(
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
150: function reconfigureFundingCyclesOf(
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
------------

## G-08 INTERNAL FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE REMOVED TO SAVE DEPLOYMENT GAS 

If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

There is 1 instance of this issue

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L32-L40

```
File: contracts/JB721GlobalGovernance.sol

32-40: function _getVotingUnits(address _account)
    internal
    view
    virtual
    override
    returns (uint256 units)
  {
    return store.votingUnitsOf(address(this), _account);
  }
```
------------
