## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Instances|
|-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 18 |
| [GAS&#x2011;2](#GAS&#x2011;2) | `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables | 7 |
| [GAS&#x2011;3](#GAS&#x2011;3) | `++i`/`i++` Should Be `unchecked{++i}`/`unchecked{i++}` When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops | 24 |
| [GAS&#x2011;4](#GAS&#x2011;4) | Use calldata instead of memory for function parameters | 19 |
| [GAS&#x2011;5](#GAS&#x2011;5) | Public Functions To External | 8 |
| [GAS&#x2011;6](#GAS&#x2011;6) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 5 |
| [GAS&#x2011;7](#GAS&#x2011;7) | Optimize names to save gas | 5 |

Total: 86 instances over 7 issues

## Gas Optimizations


### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

#### <ins>Proof Of Concept</ins>


```
44: mapping(address => mapping(uint256 => address)) internal _tierDelegation;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L44

```
53: mapping(address => mapping(uint256 => Checkpoints.History)) internal _delegateTierCheckpoints;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L53

```
57: mapping(address => mapping(uint256 => uint256)) internal _tierIdAfter;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L57

```
66: mapping(address => mapping(uint256 => address)) internal _reservedTokenBeneficiaryOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L66

```
75: mapping(address => mapping(uint256 => JBStored721Tier)) internal _storedTierOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L75

```
83: mapping(address => JBTiered721Flags) internal _flagsOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L83

```
93: mapping(address => mapping(uint256 => uint256)) internal _isTierRemoved;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L93

```
104: mapping(address => uint256) internal _trackedLastSortTierIdOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L104

```
119: mapping(address => uint256) public override maxTierIdOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L119

```
129: mapping(address => mapping(address => mapping(uint256 => uint256))) public override tierBalanceOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L129

```
138: mapping(address => mapping(uint256 => uint256)) public override numberOfReservesMintedFor;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L138

```
147: mapping(address => mapping(uint256 => uint256)) public override numberOfBurnedFor;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L147

```
155: mapping(address => address) public override defaultReservedTokenBeneficiaryOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L155

```
164: mapping(address => mapping(uint256 => address)) public override firstOwnerOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L164

```
172: mapping(address => string) public override baseUriOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L172

```
180: mapping(address => IJBTokenUriResolver) public override tokenUriResolverOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L180

```
188: mapping(address => string) public override contractUriOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L188

```
197: mapping(address => mapping(uint256 => bytes32)) public override encodedIPFSUriOf;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L197




### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables

#### <ins>Proof Of Concept</ins>


```
354: supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L354

```
409: units += _balance * _storedTierOf[_nft][_i].votingUnits;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L409

```
506: balance += tierBalanceOf[_nft][_owner][_i];
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L506

```
534: weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L534

```
563: weight +=
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L563

```
827: numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L827

```
52: carry += uint256(digits[j]) * 256;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L52





### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> `++i`/`i++` Should Be `unchecked{++i}`/`unchecked{i++}` When It Is Not Possible For Them To Overflow, As Is The Case When Used In For- And While-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas PER LOOP

#### <ins>Proof Of Concept</ins>

```
49: for (uint256 i = 0; i < _source.length; ++i) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49

```
51: for (uint256 j = 0; j < digitlength; ++j) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L51

```
68: for (uint256 i = 0; i < _length; i++) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68

```
76: for (uint256 i = 0; i < _input.length; i++) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76

```
84: for (uint256 i = 0; i < _indices.length; i++) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84




### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> Use calldata instead of memory for function parameters

In some cases, having function arguments in calldata instead of
memory is more optimal.

Consider the following generic example:
```
contract C {
	function add(uint[] memory arr) external returns (uint sum) {
		uint length = arr.length;
		for (uint i = 0; i < arr.length; i++) {
		    sum += arr[i];
		}
	}
}
```
In the above example, the dynamic array arr has the storage location
memory. When the function gets called externally, the array values are
kept in calldata and copied to memory during ABI decoding (using the
opcode calldataload and mstore). And during the for loop, arr[i]
accesses the value in memory using a mload. However, for the above
example this is inefficient. Consider the following snippet instead:
```
contract C {
	function add(uint[] calldata arr) external returns (uint sum) {
		uint length = arr.length;
		for (uint i = 0; i < arr.length; i++) {
		    sum += arr[i];
		}
	}
}
```
In the above snippet, instead of going via memory, the value is directly
read from calldata using calldataload. That is, there are no
intermediate memory operations that carries this value.

Gas savings: In the former example, the ABI decoding begins with
copying value from calldata to memory in a for loop. Each iteration
would cost at least 60 gas. In the latter example, this can be
completely avoided. This will also reduce the number of instructions and
therefore reduces the deploy time cost of the contract.

In short, use calldata instead of memory if the function argument
is only read.

Note that in older Solidity versions, changing some function arguments
from memory to calldata may cause "unimplemented feature error".
This can be avoided by using a newer (0.8.*) Solidity compiler.

Examples
Note: The following pattern is prevalent in the codebase:
```
function f(bytes memory data) external {
	(...) = abi.decode(data, (..., types, ...));
}
```
Here, changing to bytes calldata will decrease the gas. The total
savings for this change across all such uses would be quite
significant.

#### <ins>Proof Of Concept</ins>


```
function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)
    public
    virtual
    override
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L147

```
function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)
    external
    override
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L264

```
function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)
    external
    override
    onlyOwner
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290

```
function mintFor(uint16[] memory _tierIds, address _beneficiary)
    public
    override
    onlyOwner
    returns (uint256[] memory tokenIds)
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290

```
function _didBurn(uint256[] memory _tokenIds) internal override {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L598

```
function _mintAll(
    uint256 _amount,
    uint16[] memory _mintTierIds,
    address _beneficiary
  ) internal returns (uint256 leftoverAmount) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L650

```
function _redemptionWeightOf(uint256[] memory _tokenIds)
    internal
    view
    virtual
    override
    returns (uint256)
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L695

```
function tiers(
    address _nft,
    uint256 _startingId,
    uint256 _size
  ) external view override returns (JB721Tier[] memory _tiers) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L213

```
function recordAddTiers(JB721TierParams[] memory _tiersToAdd)
    external
    override
    returns (uint256[] memory tierIds)
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L628

```
function recordMintReservesFor(uint256 _tierId, uint256 _count)
    external
    override
    returns (uint256[] memory tokenIds)
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L808

```
function recordMint(
    uint256 _amount,
    uint16[] calldata _tierIds,
    bool _isManualMint
  ) external override returns (uint256[] memory tokenIds, uint256 leftoverAmount) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1012

```
function recordBurn(uint256[] memory _tokenIds) external override {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1091

```
function payParams(JBPayParamsData calldata _data)
    external
    view
    override
    returns (
      uint256 weight,
      string memory memo,
      JBPayDelegateAllocation[] memory delegateAllocations
    )
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L78

```
function redeemParams(JBRedeemParamsData calldata _data)
    external
    view
    override
    returns (
      uint256 reclaimAmount,
      string memory memo,
      JBRedemptionDelegateAllocation[] memory delegateAllocations
    )
  {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L105

```
function _didBurn(uint256[] memory _tokenIds) internal virtual {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L311

```
function _redemptionWeightOf(uint256[] memory _tokenIds) internal view virtual returns (uint256) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L323

```
function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L66

```
function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L74

```
function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L82






### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```
function setTierDelegate(address _delegatee, uint256 _tierId) public virtual override {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L177

```
function balanceOf(address _owner) public view override returns (uint256 balance) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L123

```
function tokenURI(uint256 _tokenId) public view override returns (string memory) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L138

```
function supportsInterface(bytes4 _interfaceId) public view override returns (bool) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L175


```
function mintReservesFor(uint256 _tierId, uint256 _count) public override {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L264

```
function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L499

```
function totalRedemptionWeight(address _nft) public view override returns (uint256 weight) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L550

```
function tierIdOfToken(uint256 _tokenId) public pure override returns (uint256) {
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L585






### <a href="#Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contractâ€™s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```
558: uint16[] memory _tierIdsToMint;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L558

```
46: uint8[] memory digits = new uint8[](46);
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L46

```
48: uint8 digitlength = 1;
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L48

```
67: uint8[] memory output = new uint8[](_length);
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L67

```
75: uint8[] memory output = new uint8[](_input.length);
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L75






### <a href="#Summary">[GAS&#x2011;7]</a><a name="GAS&#x2011;7"> Optimize names to save gas

`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://github.com/enzosv/solidity-optimize-name) link for an example of how it works. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

#### <ins>Proof Of Concept</ins>

```
File: \juice-nft-rewards\contracts\JB721TieredGovernance.sol
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol

```
File: \juice-nft-rewards\contracts\JBTiered721Delegate.sol
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```
File: \juice-nft-rewards\contracts\JBTiered721DelegateDeployer.sol
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol

```
File: \juice-nft-rewards\contracts\JBTiered721DelegateProjectDeployer.sol
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol

```
File: \juice-nft-rewards\contracts\JBTiered721DelegateStore.sol
```

https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol



