
 ## `_safemint()` should be used rather than `_mint()` wherever possible 
 ### Files Found: 
 There are 4 instances of this issue. 
 - File: contracts/JBTiered721Delegate.sol - line 461 
 - File: contracts/JBTiered721Delegate.sol - line 504 
 - File: contracts/JBTiered721Delegate.sol - line 635 
 - File: contracts/JBTiered721Delegate.sol - line 677 
 
 
 ### Mitigation: 
 `_mint()` is discouraged in favor of `_safeMint()` which ensures that the recipient is either an EOA or implements `IERC721Receiver`. 

 --- 