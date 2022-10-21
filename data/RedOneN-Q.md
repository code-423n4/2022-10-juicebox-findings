## [L-1] owner can renounce Ownership

The Openzeppelin’s Ownable contract implements renounceOwnership. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

***File : JBTiered721Delegate.sol***

JBTiered721Delegate.sol#L29




## [L-2] Consider two phases Ownership transfer

Admin calls Ownable.transferOwnership function to transfers the ownership to the new address directly. As such, there is a risk that the ownership is transferred to an invalid address, thus causing the contract to be without a owner.

***File : JBTiered721Delegate.sol***

JBTiered721Delegate.sol#L29




## [N-1] Constants should be defined rather than using magic numbers.

Even assembly can benefit from using readable constants instead of hex/numeric literals.

***File : JBIpfsDecoder.sol***

JBIpfsDecoder.sol#L52
JBIpfsDecoder.sol#L54
JBIpfsDecoder.sol#L60

***File : JBTiered721Delegate.sol***

JBTiered721Delegate.sol#L551

***File : JBBitmap.sol***

JBBitmap.sol#L74

***File : JBTiered721DelegateStore.sol***

JBTiered721DelegateStore.sol#L1279

***File : JBTiered721DelegateProjectDeployer.sol***

JBTiered721DelegateProjectDeployer.sol#L64




## [N-2] Lines are too long

Usually lines in source code are limited to 80 characters. Today’s screens are much larger so it’s reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

***File : JBTiered721Delegate.sol***

JBTiered721Delegate.sol#L545

***File : JBTiered721DelegateDeployer.sol***

JBTiered721DelegateDeployer.sol#L15

***File : JB721TieredGovernance.sol***

JB721TieredGovernance.sol#L235

***File : JB721Delegate.sol***

JB721Delegate.sol#L242

***File : JBTiered721DelegateProjectDeployer.sol***

JBTiered721DelegateProjectDeployer.sol#L15




## [N-3] Use of block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

***File : JBTiered721DelegateStore.sol***

JBTiered721DelegateStore.sol#L903


## [N-4] Consider using the latest solidity version 

Solidity version 0.8.17 have just been released, The latter fixes an important bug in the previous version (currently being used by Jukebox), makes overflow checks on multiplication more efficient and adds an LSP feature to always analyze all files in a project.