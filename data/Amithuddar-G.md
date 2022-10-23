1)++i costs less gas than i++, especially when it’s used in for-loops (--i/i-- too)

Saves 6 gas PER LOOP

File: juice-nft-rewards\contracts\libraries\JBIpfsDecoder.sol
  68,38:     for (uint256 i = 0; i < _length; i++) {
  76,44:     for (uint256 i = 0; i < _input.length; i++) {
  84,46:     for (uint256 i = 0; i < _indices.length; i++) {
  
2)It costs more gas to initialize variables to zero than to let the default of zero be applied

If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.
  
  
File: juice-nft-rewards\contracts\libraries\JBIpfsDecoder.sol
  49,5:     for (uint256 i = 0; i < _source.length; ++i) {
  51,7:       for (uint256 j = 0; j < digitlength; ++j) {
  68,5:     for (uint256 i = 0; i < _length; i++) {
  76,5:     for (uint256 i = 0; i < _input.length; i++) {
  84,5:     for (uint256 i = 0; i < _indices.length; i++) {  
  
3)<ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP
The overheads outlined below are PER LOOP, 

storage arrays incur a Gwarmaccess (100 gas)
memory arrays use MLOAD (3 gas)
calldata arrays use CALLDATALOAD (3 gas)
Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

File: juice-nft-rewards\contracts\libraries\JBIpfsDecoder.sol
  49,5:     for (uint256 i = 0; i < _source.length; ++i) {

  76,5:     for (uint256 i = 0; i < _input.length; i++) {
  84,5:     for (uint256 i = 0; i < _indices.length; i++) {

4)X = X + Y IS CHEAPER THAN X += Y  

File: juice-nft-rewards\contracts\JBTiered721DelegateStore.sol
  354,14:       supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;
  409,15:         units += _balance * _storedTierOf[_nft][_i].votingUnits;
  506,15:       balance += tierBalanceOf[_nft][_owner][_i];
  534,14:       weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
  563,14:       weight +=
  827,52:     numberOfReservesMintedFor[msg.sender][_tierId] += _count;
  
File: juice-nft-rewards\contracts\libraries\JBIpfsDecoder.sol
  52,15:         carry += uint256(digits[j]) * 256;

5)++i/i++ should be unchecked{++i}/unchecked{++i} when it is not possible for them to overflow,
as is the case when used in for- and while-loops  

File: juice-nft-rewards\contracts\libraries\JBIpfsDecoder.sol
  49,5:     for (uint256 i = 0; i < _source.length; ++i) {
  51,7:       for (uint256 j = 0; j < digitlength; ++j) {
  68,5:     for (uint256 i = 0; i < _length; i++) {
  76,5:     for (uint256 i = 0; i < _input.length; i++) {
  84,5:     for (uint256 i = 0; i < _indices.length; i++) {  
  
  


  