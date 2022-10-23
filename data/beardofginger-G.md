# i++ is less efficient than ++i
The gas cost can be reduced by using ++i or i += 1 instead of i++. This saves about 5 gas.

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



