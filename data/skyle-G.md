[G-01]INCREMENTS/DECREMENTS CAN BE UNCHECKED IN FOR-LOOPS

JBIpfsDecoder.sol
  Ln 49 for (uint256 i = 0; i < _source.length; ++i)

G-03 <ARRAY>.LENGTH SHOULD NOT BE LOOKED UP IN EVERY LOOP OF A FOR-LOOP

JBIpfsDecoder.sol
  Ln 49 for (uint256 i = 0; i < _source.length; ++i)
  Ln 76 for (uint256 i = 0; i < _input.length; i++) {

[G-4] IT COSTS MORE GAS TO INITIALIZE VARIABLES WITH THEIR DEFAULT VALUE THAN LETTING THE DEFAULT VALUE BE APPLIED.

JBIpfsDecoder.sol
  Ln 49 for (uint256 i = 0; i < _source.length; ++i)
  Ln 51 for (uint256 j = 0; j < digitlength; ++j) {
  Ln 68 for (uint256 i = 0; i < _length; i++) {
  Ln 76 for (uint256 i = 0; i < _input.length; i++) {
  Ln 84 for (uint256 i = 0; i < _indices.length; i++) {

G-05 ++i costs less gas compared to i++ or i += 1 (same for --i vs i-- or i -= 1)

JBIpfsDecoder.sol
  Ln 68 for (uint256 i = 0; i < _length; i++) {
  Ln 76 for (uint256 i = 0; i < _input.length; i++) {
  Ln 84 for (uint256 i = 0; i < _indices.length; i++) {


