# Table of contents

- **[[L-01] `initialize() is prone to front-running`](L-01)**
- **[[L-02] `ERC721.mint()` doesn't check that the contract is able to receive ERC721 tokens](L-02)**


## **[L-01] `initialize() is prone to front-running`**<a name="L-01"></a>

### ***Description:***
  - Mev botes actually can front-run initialization process of a `JBTiered721Delegate.sol`. That will require full redeploying process. 

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: contracts/JBTiered721Delegate.sol 
      ........................................
      
        // Lines: [202-252]
          function initialize(
            uint256 _projectId,
            IJBDirectory _directory,
            string memory _name,
            string memory _symbol,
            IJBFundingCycleStore _fundingCycleStore,
            string memory _baseUri,
            IJBTokenUriResolver _tokenUriResolver,
            string memory _contractUri,
            JB721PricingParams memory _pricing,
            IJBTiered721DelegateStore _store,
            JBTiered721Flags memory _flags
          ) public override {
            // Make the original un-initializable.
            require(address(this) != codeOrigin);
            // Stop re-initialization.
            require(address(store) == address(0));

            // Initialize the sub class.
            JB721Delegate._initialize(_projectId, _directory, _name, _symbol);

            fundingCycleStore = _fundingCycleStore;
            store = _store;
            pricingCurrency = _pricing.currency;
            pricingDecimals = _pricing.decimals;
            prices = _pricing.prices;

            // Store the base URI if provided.
            if (bytes(_baseUri).length != 0) _store.recordSetBaseUri(_baseUri);

            // Set the contract URI if provided.
            if (bytes(_contractUri).length != 0) _store.recordSetContractUri(_contractUri);

            // Set the token URI resolver if provided.
            if (_tokenUriResolver != IJBTokenUriResolver(address(0)))
              _store.recordSetTokenUriResolver(_tokenUriResolver);

            // Record adding the provided tiers.
            if (_pricing.tiers.length > 0) _store.recordAddTiers(_pricing.tiers);

            // Set the flags if needed.
            if (
              _flags.lockReservedTokenChanges ||
              _flags.lockVotingUnitChanges ||
              _flags.lockManualMintingChanges ||
              _flags.pausable
            ) _store.recordFlags(_flags);

            // Transfer ownership to the initializer.
            _transferOwnership(msg.sender);
          }

### ***Recommendations:***

## **[L-02] `ERC721.mint()` doesn't check that the contract is able to receive ERC721 tokens**<a name="L-02"></a>

### ***Description:***
  - If the receiver is not an EOA and doesn't implement the callback to handle nft's. ERC721 tokens are lost forever. 

### ***All occurances:***

  - Contracts:
  
    ```Solidity
      file: contracts/JBTiered721Delegate.sol 
      ........................................
      
        // Lines: [480-512]
          function mintFor(uint16[] memory _tierIds, address _beneficiary)
            public
            override
            onlyOwner
            returns (uint256[] memory tokenIds)
          {
            // Record the mint. The returned token IDs correspond to the tiers passed in.
            (tokenIds, ) = store.recordMint(
              type(uint256).max, // force the mint.
              _tierIds,
              true // manual mint
            );

            // Keep a reference to the number of tokens being minted.
            uint256 _numberOfTokens = _tierIds.length;

            // Keep a reference to the token ID being iterated on.
            uint256 _tokenId;

            for (uint256 _i; _i < _numberOfTokens; ) {
              // Set the token ID.
              _tokenId = tokenIds[_i];

              // Mint the token.
              _mint(_beneficiary, _tokenId);

              emit Mint(_tokenId, _tierIds[_i], _beneficiary, 0, msg.sender);

              unchecked {
                ++_i;
              }
            }
          }

### ***Recommendations:***
  - Try `_safeMint()` instead or manually check whether the callback exists.