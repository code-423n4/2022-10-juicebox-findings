1.

Grammer issues in comments in:

1.1

Contract: JB721GlobalGovernance.sol

1.1.1

	line 12

	"on chain" should be hyphened "on-chain"
	
Modified Comment:

	A tiered 721 delegate where each NFT can be used for on-chain governance, with votes delegatable globally across all tiers.

1.1.2

	line 48

	"transfered" is misspelled and should be "transferred"

Modified Comment:

	@param _tokenId The id of the token for which voting units are being transferred.

1.2

Contract: JBTiered721DelegateDeployer.sol

1.2.1

	line 26

	the "that" before "support" is unnecessary

Modified Comment:

	The contract supports on-chain governance across all tiers. 

1.2.2

	line 32

	"per-tier" should not be hyphenated

Modified Comment:

	The contract that supports on-chain governance per tier. 

1.2.3

	line 38
	 
	the "that" before "has" is unnecessary

Modified Comment:

	The contract has no on-chain governance. 

1.2.4
	
	line 64

	there should be a "to" in front of "this"

Modified Comment:

	@param _projectId The ID of the project to this contract's functionality applies to.

1.2.5

	line 120

	consider adding a "which is" in front of not for better readability

Modified Comment:
	
	// Get a bit of freemem to land the bytecode, which is not updated as we'll leave this scope right after create(..)

1.3
	
Contract: JBTiered721DelegateProjectDeployer.sol

1.3.1

	line 19

	the comma after "owner" is unnecessary

	"preconfifigured" is misspelled and should be "preconfigured"

Modified Comment:

	JBOperatable: Several functions in this contract can only be accessed by a project owner or an address that has been preconfigured to be an operator of the project.
	
1.3.2
	
	line 34

	"responsibile" is misspelled and should be "responsible"

Modified Comment:

	The contract responsible for deploying the delegate. 
	
1.3.3
	
	lines 148 and 240

	the "that" before "was" is unnecessary

Modified Comments:

	@return configuration The configuration of the funding cycle was successfully reconfigured.

	@return The configuration of the funding cycle was successfully reconfigured.

1.4
	
Contract: JB721TieredGovernance.sol

1.4.1

	line 13

	"on chain" should be hyphened "on-chain"

Modified Comment:

	A tiered 721 delegate where each NFT can be used for on-chain governance, with votes delegatable per tier.

1.4.2

	line 69

	there should be an "a" before "specific"

Modified Comment:

	Returns the delegate of an account for a specific tier.

1.4.3

	line 71

	the "of" after "delegate" is unnecessary

Modified Comment:

	@param _account The account to check for a delegate.

1.4.4

	line 209, 239, 269, and 306

Modified Comments:

	"transfered" is misspelled and should be "transferred"
 
	@param _tierId The ID of the tier for which voting units are being transferred.

	@param _tierId The ID of the tier for which voting units are being transferred.

	@param _tierId The ID of the tier for which voting units are being transferred.

	@param _tokenId The ID of the token for which voting units are being transferred.


1.4.5

	line 235

	there should be a "The" in front of "Total"

Modified Comment:

	Transfers, mints, or burns tier voting units. To register a mint, `from` should be zero. To register a burn, `to` should be zero. The total supply of voting units will be adjusted with mints and burns.

1.5
	
Contract: JBTiered721Delegate.sol

1.5.1

	line 17

	"base" is stated twice after "assets" and is only needed once

Modified Comment:

	Delegate that offers project contributors NFTs with tiered price floors upon payment and the ability to redeem NFTs for treasury assets based on price floor.

1.5.2

	line 46

	there should be a "was" before "used"

Modified Comment:

	The address of the origin 'JBTiered721Delegate', was used to check in the init if the contract is the original or not

1.5.3

	line 52

	the "that" in front of "stores" is unnecessary

Modified Comment:

	The contract stores and manages the NFT's data.

1.5.4

	line 58

	"storing" should be "stores"

Modified Comment:

	The contract stores all funding cycle configurations.

1.5.5

	line 64

	the "that" before "exposes" is unnecessary

Modified Comment:

	The contract exposes price feeds.

1.5.6

	line 82

	"contribute" should be "contributed"

Modified Comment:

	The amount that each address has paid that has not yet contributed to the minting of an NFT. 

1.5.7

	line 94

	"which" before "corresponds" is unnecessary

Modified Comment:

	The first owner of each token ID, corresponds to the address that originally contributed to the project to receive the NFT.

1.5.8

	line 121

	"accross" is misspelled and should be "across"

Modified Comment:

	@return balance The number of tokens owners by the owner across all tiers.
	
1.5.9	
	
	line 173

	"adherance" is misspelled and should be "adherence"

Modified Comment:

	@param _interfaceId The ID of the interface to check for adherence to.

1.5.10

	line 190

	there should be a "to" before "this"

Modified Comment:

	@param _projectId The ID of the project to this contract's functionality applies to.

1.5.11

	line 198

	"tier" should be "tiered"

Modified Comment:

	@param _pricing The tiered pricing according to which token distribution will be made. Must be passed in order of contribution floor, with implied increasing value.

1.5.12

	line 220

	"sub class" should be hyphenated "sub-class"

Modified Comment:

	// Initialize the sub-class.

1.5.13

	line 262

	there should be a "the" before "mint"

Modified Comment:

	@param _mintReservesForTiersData Contains information about how many reserved tokens to the mint for each tier.

1.5.14

	line 288

	the "who" before "to mint" is unnecessary

	the "for" before "from" is unnecessary

Modified Comment:

	@param _mintForTiersData Contains information about how to mint tokens for from each tier.

1.5.15

	line 295, and 363
	
	"beneificiary" is misspelled and should be "beneficiary"

Modified Comments:

	// Keep a reference to the number of beneficiaries there are to the mint for.

	Sets the beneficiary of the reserved tokens for tiers where a specific beneficiary isn't set. 

1.5.17

	line 368

Modified Comment:

	@param _beneficiary The default beneficiary of the reserved tokens.

1.5.18

	line 545

	"provded" is misspelled and should be "provided"

Modified Comment:

	// Keep a reference to the flag indicating if the transaction should revert if all provided funds aren't spent. Defaults to false, meaning only a minimum payment is enforced.

1.5.19

	line 557

	the second "the" before "specific" is unnecessary

Modified Comment:

	// Keep a reference to the specific tier IDs to mint.

1.5.20

	line 574

	"leftover" should be two words, "left over"
	
Modified Comment:

	// If there are funds left over, mint the best available with it.

1.5.21

	line 594

	the "a" before "tokens" are unnecessary

Modified Comment:

	A function that will run when tokens are burned via redemption.

1.5.24

	line 691

	the "of" after "weight" is unnecessary

Modified Comment:
	
	@param _tokenIds The IDs of the tokens to get the cumulative redemption weight.

1.5.25

	line 717

	"regitered" is misspelled and should be "registered"

Modified Comment:

	User the hook to register the first owner if it's not yet registered.

1.5.26

	line 721, 728, 757, and 782
	
	"transfered" is misspelled and should be "transferred"

Modified Comments:

	@param _tokenId The ID of the token being transferred.

	// Transferred must not be paused when not minting or burning.

	@param _tokenId The ID of the token being transferred.

	@param _tokenId The ID of the token for which voting units are being transferred.

1.6
	
Contract: JBTiered721DelegateStore.sol

1.6.1

	line 15

	the "that" before "stores" is unnecessary

Modified Comment:

	The contract stores and manages the NFT's data.

1.6.2

	line 87

	"indicating" should be "indicates"

Modified Comment:

	For each tier ID, a flag indicates if the tier has been removed. 

1.6.3

	line 89

	"belong" should be "belongs"

Modified Comment:

	_nft The NFT contract to which the tier belongs.

1.6.4

	line 123

	there should be an "is" before "within"

Modified Comment:

	Each account's balance is within a specific tier.

1.6.5

	line 159

	there should be an "is" before "stored"

	there should be a "the" before "first"

Modified Comment:

	The first owner of each token ID, is stored on the first transfer out.

1.6.6

	line 230

	"referecen" is misspelled and should be "reference"

Modified Comment:

	// Keep a reference to the tier being iterated on.

1.6.7

	line 303

	there should be a "the" before "token"

Modified Comment:

	@param _tokenId The ID of the token to return the tier of. 

1.6.8

	line 497

	"accross" is misspelled and should be "across"

Modified Comment:

	@return balance The number of tokens owners by the owner across all tiers.

1.6.9

	line 519

	the "of" after "weight" is unnecessary

Modified Comment:

	@param _tokenIds The IDs of the tokens to get the cumulative redemption weight.

1.6.10

	line 579, and 951

	"Tier's" should be "Tiers"

Modified Comments:
	
	Tiers are 1 indexed from the `tiers` array, meaning the 0th element of the array is tier 1.

	// Set the tier being iterated on. Tiers are 1 indexed.
	
1.6.11

	line 581

	the "of" after "number" is unnecessary

Modified Comment:

	@param _tokenId The ID of the token to get the tier number. 

1.6.12

	line 597

	"benficiary" is misspelled and should be "beneficiary"

Modified Comment:

	@return The reserved token beneficiary.
	
1.6.13	
	
	line 719

	"idex" is misspelled and should be "index"

Modified Comment:

	// Keep a reference to the index to iterate on next.

1.6.14

	line 745

	there should be a "the" before "index"

Modified Comment:

	// If the previous after the index was set to something else, set the previous after.

1.6.15

	line 770

	the "a" before "last" should be "the"

Modified Comment:

	// If there's currently the last sort index tracked, override it.

1.6.16

	line 776

	the "be" before "the current" is unnecessary

Modified Comment:

	// Set the previous index to the current index.

1.6.17

	line 852
	
	"reservd" is misspelled and should be "reserved"

Modified Comment:

	@param _beneficiary The reserved token beneficiary.
	
1.6.18	
	
	line 862
	
	consider adding a "of" before "the tier" for better readability
	
	"transfered" is misspelled and should be "transferred"

Modified Comment:

	@param _tierId The ID of the tier being transferred

1.6.19

	line 920

	there should be a "was" before "minted"

Modified Comment:

	@return tokenId The token ID was minted.
	
1.6.20	
	
	line 1250

	"non reserved" should be hyphenated to "non-reserved"

Modified Comment:

	// Get the number of reserved tokens mintable given the number of non-reserved tokens minted. This will round down.

1.6.21

	line 1253

	"Round up" should be one word, "Roundup"

Modified Comment:

	// Roundup.

1.6.22

	line 1297

	there should be a "the" before "current"

Modified Comment:

	// If this is the last tier, set the current to zero to break out of the loop.

1.7

Contract: JB721Delegate.sol

1.7.1

	line 54
	
	there should be a "to" before "this"

	the "to" after "applies" is unnecessary

Modified Comment:

	The ID of the project to this contract's functionality applies.

1.7.2

	line 88

	"recieved" is misspelled and should be "received"

	"pay" should be "paid"

Modified Comment:

	// Forward the received weight and memo, and use this contract as a paid delegate.

1.7.3

	line 176

	"adherance" is misspelled and should be "adherence"

	the "to" after "adherence" is unnecessary

Modified Comment:

	@param _interfaceId The ID of the interface to check for adherence.

1.7.4

	line 198

	there should be a "to" before "this"

Modified Comment:

	@param _projectId The ID of the project to this contract's functionality applies to.

1.7.5

	lines 229, and 250

	the "an" before "interaction" is unnecessary

Modified Comments:

	// Make sure the caller is a terminal of the project, and the call is being made on behalf of interaction with the correct project.
	
	the above comment is the same for both lines.

1.7.6

	line 307

	the "a" before "tokens" is unnecessary
 
Modified Comment: 
 
	A function that will run when tokens are burned via redemption.

1.7.7

	line 319
	
	the "of" after "weight" is unnecessary
	
Modified Comment:	
	
	@param _tokenIds The IDs of the tokens to get the cumulative redemption weight.

1.8

Contract: JBBitmap.sol

1.8.1

	line 57

	the "an" before "another" is unnecessary

Modified Comment:

	Return true if the index is in another word than the one stored in the BitmapWord struct.

1.9

Contract: JBIpfsDecoder.sol

1.9.1

	line 30

	the "an" before "hash" should be "a"

Modified Comment:

	// Convert the hex string to a hash

1.9.2

	line 39

	the "an" before "hex" should be "a"

Modified Comment:

	Convert a hex string to base58
	
2.

Comments overflow in the GitHub repository which interferes with the cleanliness of code and makes readability harder:

2.1

Contract: JBTiered721DelegateDeployer.sol

2.1.1

	line 15

Modified Comment Layout:

	IJBTiered721DelegateDeployer: General interface for the generic controller methods in this contract that interacts 
	with funding cycles and tokens according to the protocol's rules.

2.2

Contract: JBTiered721DelegateProjectDeployer.sol

2.2.1

	line 15

Modified Comment Layout:

	IJBTiered721DelegateProjectDeployer: General interface for the generic controller methods in this contract that interacts 
	with funding cycles and tokens according to the protocol's rules.

2.2.2

	line19

Modified Comment Layout:

	JBOperatable: Several functions in this contract can only be accessed by a project owner, 
	or an address that has been preconfifigured to be an operator of the project.
	
Note:

	If the previous grammar step were followed for this line the comment will become
	
	JBOperatable: Several functions in this contract can only be accessed by a project owner 
	or an address that has been preconfigured to be an operator of the project.


2.3
	
Contract: JB721TieredGovernance.sol

2.3.1

	line 235

Modified Comment Layout:

	Transfers, mints, or burns tier voting units. To register a mint, `from` should be zero. To register a burn, `to` should be zero. 
	Total supply of voting units will be adjusted with mints and burns.
	
Note:

	If the previous grammar step were followed for this line the comment will become
	
	Transfers, mints, or burns tier voting units. To register a mint, `from` should be zero. To register a burn, `to` should be zero. 
	The total supply of voting units will be adjusted with mints and burns.

2.4

Contract: JBTiered721Delegate.sol

2.4.1

	line 17

Modified Comment Layout:

	Delegate that offers project contributors NFTs with tiered price floors upon payment and the ability to redeem NFTs for 
	treasury assets based based on price floor.
	
Note:

	If the previous grammar step were followed for this line the comment will become
	
	Delegate that offers project contributors NFTs with tiered price floors upon payment and the ability to redeem NFTs for 
	treasury assets based on price floor.
	
2.4.2	
	
	line 198

Modified Comment Layout:

	@param _pricing The tier pricing according to which token distribution will be made. Must be passed in order of contribution floor, 
	with implied increasing value.
	
Note:

	If the previous grammar step were followed for this line the comment will become
	
	@param _pricing The tiered pricing according to which token distribution will be made. 
	Must be passed in order of contribution floor, with implied increasing value.

2.4.3

	line 545

Modified Comment Layout:

	// Keep a reference to the flag indicating if the transaction should revert if all provded funds aren't spent. 
	// Defaults to false, meaning only a minimum payment is enforced.
	
Note:

	If the previous grammar step were followed for this line the comment will become
	
	// Keep a reference to the flag indicating if the transaction should revert if all provided funds aren't spent. 
	// Defaults to false, meaning only a minimum payment is enforced.

2.5

Contract: JB721Delegate.sol

2.5.1

	line 70

Modified Comment Layout:

	Part of IJBFundingCycleDataSource, this function gets called when the project receives a payment. 
	It will set itself as the delegate to get a callback from the terminal.

2.5.2

	line 221

Modified Comment Layout:

	Part of IJBPayDelegate, this function gets called when the project receives a payment. 
	It will mint an NFT to the contributor (_data.beneficiary) if conditions are met.

2.5.3

	line 242

Modified Comment Layout:

	Part of IJBRedeemDelegate, this function gets called when the token holder redeems. 
	It will burn the specified NFTs to reclaim from the treasury to the _data.beneficiary.



