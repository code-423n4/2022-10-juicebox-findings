# No Transfer ownership pattern

```
File: JBTiered721Delegate.sol
5: import '@openzeppelin/contracts/access/Ownable.sol';
```

The current (openzeppelin) ownership transfer process involves the current owner calling transferOwnership(). This function checks the new owner is not the zero address and proceeds to write the new owner’s address into the owner’s state variable. If the nominated EOA account is not a valid account, it is entirely possible the owner may accidentally transfer ownership to an uncontrolled account, breaking all functions with the onlyOwner() modifier.

Implement zero address check and consider implementing a two step process where the owner nominates an account and the nominated account needs to call an acceptOwnership() function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account.


# Verify if new input is not the same as existing state variable

We can check if the new _baseUri is not the same as current _baseUri, (same with _contractUri)
```
File: JBTiered721Delegate.sol
386:   function setBaseUri(string memory _baseUri) external override onlyOwner {
387:     // Store the new value.
388:     store.recordSetBaseUri(_baseUri);
389: 
390:     emit SetBaseUri(_baseUri, msg.sender);
391:   }

File: JBTiered721Delegate.sol
402:   function setContractUri(string calldata _contractUri) external override onlyOwner {
403:     // Store the new value.
404:     store.recordSetContractUri(_contractUri);
405: 
406:     emit SetContractUri(_contractUri, msg.sender);
407:   }

File: JBTiered721Delegate.sol
418:   function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {
419:     // Store the new value.
420:     store.recordSetTokenUriResolver(_tokenUriResolver);
421: 
422:     emit SetTokenUriResolver(_tokenUriResolver, msg.sender);
423:   }

```

