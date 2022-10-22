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

# 7. [G-7] Arithmetics: uncheck blocks for arithmetics operations that can't underflow/overflow

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn't possible, some gas can be saved by using an `unchecked` block.

I suggest wrapping with an `unchecked` block here:

    File contracts/JB721TieredGovernance.sol, line 323:       return a + b;
    File contracts/JB721TieredGovernance.sol, line 327:       return a - b;

    File contracts/JBTiered721Delegate.sol, line 529:       
            _value = PRBMath.mulDiv(
                _data.amount.value,
                10**pricingDecimals,
                prices.priceFor(_data.amount.currency, pricingCurrency, _data.amount.decimals)
            );
    File contracts/JBTiered721Delegate.sol, line 540:     uint256 _leftoverAmount = _value + _credits;  

    File contracts/JBTiered721DelegateStore.sol, line 354:       supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;
    File contracts/JBTiered721DelegateStore.sol, line 409:       units += _balance * _storedTierOf[_nft][_i].votingUnits;
    File contracts/JBTiered721DelegateStore.sol, line 438:       return _balance * _storedTierOf[_nft][_tierId].votingUnits;
    File contracts/JBTiered721DelegateStore.sol, line 506:       balance += tierBalanceOf[_nft][_owner][_i];
    File contracts/JBTiered721DelegateStore.sol, line 563:       
            weight +=
                (_storedTier.contributionFloor *
                (_storedTier.initialQuantity - _storedTier.remainingQuantity)) +
                _numberOfReservedTokensOutstandingFor(_nft, _i, _storedTier);
    File contracts/JBTiered721DelegateStore.sol, line 685:       uint256 _tierId = _currentMaxTierIdOf + _i + 1;
    File contracts/JBTiered721DelegateStore.sol, line 827:       numberOfReservesMintedFor[msg.sender][_tierId] += _count;
    File contracts/JBTiered721DelegateStore.sol, line 874:       --tierBalanceOf[msg.sender][_from][_tierId];
    File contracts/JBTiered721DelegateStore.sol, line 837-840:       
            tokenIds[_i] = _generateTokenId(
                _tierId,
                _storedTier.initialQuantity - --_storedTier.remainingQuantity + _numberOfBurnedFromTier
            );
    File contracts/JBTiered721DelegateStore.sol, line 997:       leftoverAmount = _amount - _bestContributionFloor;
    File contracts/JBTiered721DelegateStore.sol, line 1077:       leftoverAmount = leftoverAmount - _storedTier.contributionFloor;
    File contracts/JBTiered721DelegateStore.sol, line 1106:       numberOfBurnedFor[msg.sender][_tierId]++;
    File contracts/JBTiered721DelegateStore.sol, line 1248:       uint256 _numerator = uint256(_numberOfNonReservesMinted * _storedTier.reservedRate);
    File contracts/JBTiered721DelegateStore.sol, line 1251:       uint256 _numberReservedTokensMintable = _numerator / JBConstants.MAX_RESERVED_RATE;
    File contracts/JBTiered721DelegateStore.sol, line 1255:       ++_numberReservedTokensMintable;
    File contracts/JBTiered721DelegateStore.sol, line 1258:       return _numberReservedTokensMintable - _reserveTokensMinted;

# 8. [G-8] Variables: Cache read variables in memory will save gas

File contracts/JBTiered721DelegateStore.sol, line 344-359:

            JBStored721Tier storage _storedTier;

            // Keep a reference to the greatest tier ID.
            uint256 _maxTierId = maxTierIdOf[_nft];

            for (uint256 _i = _maxTierId; _i != 0; ) {
                    // Set the tier being iterated on.
                    _storedTier = _storedTierOf[_nft][_i];

                    // Increment the total supply with the amount used already.
                    supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;

                    unchecked {
                        --_i;
                    }
            }

I suggest code replace:

            JBStored721Tier memory _storedTier;
            mapping(address => mapping(uint256 => JBStored721Tier)) memory storedTierOf =  _storedTierOf;

            // Keep a reference to the greatest tier ID.
            uint256 _maxTierId = maxTierIdOf[_nft];

            for (uint256 _i = _maxTierId; _i != 0; ) {
                    // Set the tier being iterated on.
                    _storedTier = storedTierOf [_nft][_i];

                    // Increment the total supply with the amount used already.
                    supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;

                    unchecked {
                        --_i;
                    }
            }

File contracts/JBTiered721DelegateStore.sol, line 403-414:

            for (uint256 _i = _maxTierId; _i != 0; ) {
                    // Get a reference to the account's balance in this tier.
                    _balance = tierBalanceOf[_nft][_account][_i];
                    if (_balance != 0)
                        // Add the tier's voting units.
                        units += _balance * _storedTierOf[_nft][_i].votingUnits; 

                    unchecked {
                        --_i;
                    }
            }

I suggest code replace:

            mapping(address => mapping(address => mapping(uint256 => uint256))) memory _tierBalanceOf = tierBalanceOf;
            mapping(address => mapping(uint256 => JBStored721Tier)) memory storedTierOf =  _storedTierOf;

            for (uint256 _i = _maxTierId; _i != 0; ) {
                        // Get a reference to the account's balance in this tier.
                        _balance = _tierBalanceOf [_nft][_account][_i];
                        if (_balance != 0)
                            // Add the tier's voting units.
                            units += _balance * storedTierOf[_nft][_i].votingUnits; 

                        unchecked {
                            --_i;
                        }
            }

File contracts/JBTiered721DelegateStore.sol, line 504-511:

            for (uint256 _i = _maxTierId; _i != 0; ) {
                    // Get a reference to the account's balance in this tier.
                    balance += tierBalanceOf[_nft][_owner][_i];

                    unchecked {
                        --_i;
                    }
            }

I suggest code replace:

            mapping(address => mapping(address => mapping(uint256 => uint256))) memory _tierBalanceOf = tierBalanceOf;
            for (uint256 _i = _maxTierId; _i != 0; ) {
                    // Get a reference to the account's balance in this tier.
                    balance += _tierBalanceOf[_nft][_owner][_i];

                    unchecked {
                        --_i;
                    }
            }

File contracts/JBTiered721DelegateStore.sol, line 533-539:

            for (uint256 _i; _i < _numberOfTokenIds; ) {
                    weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;

                    unchecked {
                        ++_i;
                    }
            }

I suggest code replace:

            mapping(address => mapping(uint256 => JBStored721Tier)) memory storedTierOf =  _storedTierOf;
            for (uint256 _i; _i < _numberOfTokenIds; ) {
                    weight += storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;

                    unchecked {
                        ++_i;
                    }
            }

File contracts/JBTiered721DelegateStore.sol, line 558-571:

            for (uint256 _i; _i < _maxTierId; ) {
                    _storedTier = _storedTierOf[_nft][_i + 1];

                    ...
            }

I suggest code replace:

            mapping(address => mapping(uint256 => JBStored721Tier)) memory storedTierOf =  _storedTierOf;
            for (uint256 _i; _i < _numberOfTokenIds; ) {
                    _storedTier = storedTierOf[_nft][_i + 1];

                    ...
            }

File contracts/JBTiered721DelegateStore.sol, line 898-911:

            for (uint256 _i; _i < _numTiers; ) {
                    ...
                    if (_storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();
                    ...
            }

I suggest code replace:

            mapping(address => mapping(uint256 => JBStored721Tier)) memory storedTierOf =  _storedTierOf;
            for (uint256 _i; _i < _numTiers; ) {
                    ...
                    if (storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();
                    ...
            }