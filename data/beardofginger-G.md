# > 0 is less efficient than != 0 for unsigned integers
The gas cost can be reduced by using != 0 instead of > 0 in the following locations:

JB721Delegate.sol, Line 116:

		if (_data.tokenCount > 0) revert UNEXPECTED_TOKEN_REDEEMED();

JBIpfsDecoder.sol, Line 57:

		while (carry > 0) {

JBTiered721Delegate.sol, Line 240:

		if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);

JBTiered721DelegateStore.sol, Line 1254:

		if (_numerator - JBConstants.MAX_RESERVED_RATE * _numberReservedTokensMintable > 0)

# i++ is less efficient than ++i
The gas cost can be reduced by using ++i or i += 1 instead of i++ in the following locations:

JBIpfsDecoder.sol, Line 59:

		digitlength++;

JBIpfsDecoder.sol, Line 68:

		for (uint256 i = 0; i < _length; i++) {

JBIpfsDecoder.sol, Line 76:

		for (uint256 i = 0; i < _input.length; i++) {

JBIpfsDecoder.sol, Line 84:

		for (uint256 i = 0; i < _indices.length; i++) {

JBTiered721DelegateStore.sol, Line 245:

		_tiers[_numberOfIncludedTiers++] = JB721Tier({

JBTiered721DelegateStore.sol, Line 1108:

		_storedTierOf[msg.sender][_tierId].remainingQuantity++;

# Initialising variables to 0 uses unecessary gas
Uninitialsed variables default to 0x0. Hence, assigning them 0 uses gas unecessarily.

JBIpfsDecoder.sol, Line 49:

		for (uint256 i = 0; i < _source.length; ++i) {

JBIpfsDecoder.sol, Line 51:

		for (uint256 j = 0; j < digitlength; ++j) {

JBIpfsDecoder.sol, Line 68:

		for (uint256 i = 0; i < _length; i++) {

JBIpfsDecoder.sol, Line 76:

		for (uint256 i = 0; i < _input.length; i++) {

JBIpfsDecoder.sol, Line 84:

		for (uint256 i = 0; i < _indices.length; i++) {
