## [NAZ-L1] Missing Time locks
**Severity**: Low 
**Context**: [`JBTiered721Delegate.sol#L321`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L321), [`JBTiered721Delegate.sol#L370`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L370), [`JBTiered721Delegate.sol#L386`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L386), [`JBTiered721Delegate.sol#L402`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L402), [`JBTiered721Delegate.sol#L418`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L418)

**Description**:
When critical parameters of systems need to be changed, it is required to broadcast the change via event emission and recommended to enforce the changes after a time-delay. This is to allow system users to be aware of such critical changes and give them an opportunity to exit or adjust their engagement with the system accordingly. None of the onlyOwner functions that change critical protocol addresses/parameters have a timelock for a time-delayed change to alert: (1) users and give them a chance to engage/exit protocol if they are not agreeable to the changes (2) team in case of compromised owner(s) and give them a chance to perform incident response.

**Recommendation**:
Users may be surprised when critical parameters are changed. Furthermore, it can erode users' trust since they can’t be sure the protocol rules won’t be changed later on. Compromised owner keys may be used to change protocol addresses/parameters to benefit attackers. Without a time-delay, authorised owners have no time for any planned incident response.


## [NAZ-L2] Missing Zero-address Validation
**Severity**: Low
**Context**: [`JB721Delegate.sol#L203`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L203), [`JBTiered721Delegate.sol#L202`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L202)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-L3] Missing Equivalence Checks in Setters
**Severity**: Low
**Context**: [`JBTiered721Delegate.sol#L321`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L321), [`JBTiered721Delegate.sol#L370`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L370), [`JBTiered721Delegate.sol#L386`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L386), [`JBTiered721Delegate.sol#L402`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L402), [`JBTiered721Delegate.sol#L418`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L418)

**Description**:
Setter functions are missing checks to validate if the new value being set is the same as the current value already set in the contract. Such checks will showcase mismatches between on-chain and off-chain states.

**Recommendation**:
This may hinder detecting discrepancies between on-chain and off-chain states leading to flawed assumptions of on-chain state and protocol behavior.


## [NAZ-L4] Missing Events In Initialize Functions
**Severity**: Low
**Context**: [`JB721Delegate.sol#L203`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L203), [`JBTiered721Delegate.sol#L202`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L202)

**Description**:
None of the initialize functions emit emit init-specific events. They all however have the initializer modifier (from Initializable) so that they can be called only once. Off-chain monitoring of calls to these critical functions is not possible.

**Recommendation**:
It is recommended to emit events in your initialization functions.


## [NAZ-N1] Function States Named Return But Doesn't Use It
**Severity** Informational
**Context**: [`JB721Delegate.sol#L110-L111`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L110-L111), [`JB721GlobalGovernance.sol#L37`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L37), [`JBTiered721Delegate.sol#L123`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L123), [`JBTiered721DelegateProjectDeployer.sol#L119`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L119), [`JBTiered721DelegateProjectDeployer.sol#L162`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L162)

**Description**:
The linked function(s) have a named return that is not used. Using both named returns and a return statement isn't necessary. 

**Recommendation**:
Removing one of those can improve code clarity.


## [NAZ-N2] Line Length
**Severity**: Informational
**Context**: [`JB721Delegate.sol#L221`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L221), [`JB721Delegate.sol#L229`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L229), [`JB721Delegate.sol#L242`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L242), [`JB721Delegate.sol#L250`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L250), [`JB721TieredGovernance.sol#L235`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L235), [`JBBitmap.sol#L37`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L37), [`JBTiered721Delegate.sol#L17`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L17), [`JBTiered721Delegate.sol#L21`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L21), [`JBTiered721Delegate.sol#L27`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L27), [`JBTiered721Delegate.sol#L94`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L94), [`JBTiered721Delegate.sol#L198`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L198), [`JBTiered721Delegate.sol#L542`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#l542), [`JBTiered721Delegate.sol#L545`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L545), [`JBTiered721Delegate.sol#L548`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L548), [`JBTiered721DelegateDeployer.sol#L15`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateDeployer.sol#L15)

**Description**:
Max line length must be no more than 120 but many lines are extended past this length.

**Recommendation**:
Consider cutting down the line length below 120.


## [NAZ-N3] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`JBBitmap.sol#L73`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L73)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N4] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`JBBitmap.sol#L37`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L37)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N5] Unclear Revert Messages
**Severity** Informational
**Context**: [`JBTiered721Delegate.sol#L216`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L216), [`JBTiered721Delegate.sol#L218`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L218)

**Description**:
Some revert messages are unclear which can lead to confusion. Unclear revert messages may cause misunderstandings on reverted transactions.

**Recommendation**: 
Consider making revert messages more clear.


## [NAZ-N6] Spelling Errors
**Severity**: Informational
**Context**: [`JB721Delegate.sol#L88 (recieved => received)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L88), [`JB721Delegate.sol#L176 (adherance => adherence)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol#L176), [`JB721GlobalGovernance.sol#L48 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L48), [`JB721TieredGovernance.sol#L209 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L209), [`JB721TieredGovernance.sol#L239 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L239), [`JB721TieredGovernance.sol#L269 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L269), [`JB721TieredGovernance.sol#L306 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L306), [`JBBitmap.sol#L13 (initialse => initialize)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol#L13), [`JBTiered721Delegate.sol#L121 (accross => across)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L121), [`JBTiered721Delegate.sol#L173 (adherance => adherence)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L173), [`JBTiered721Delegate.sol#L288 (how who => who)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L288), [`JBTiered721Delegate.sol#L545 (provded => provided)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L545), [`JBTiered721Delegate.sol#L717 (regitered => registered)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L717), [`JBTiered721Delegate.sol#L721 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L721), [`JBTiered721Delegate.sol#L757 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L757), [`JBTiered721Delegate.sol#L782 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L782), [`JBTiered721DelegateProjectDeployer.sol#L19 (preconfifigured => preconfigured)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L19), [`JBTiered721DelegateProjectDeployer.sol#L34 (responsibile => responsible)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol#L34), [`JBTiered721DelegateStore.sol#L176 (superceeds => supersede)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L176), [`JBTiered721DelegateStore.sol#L230 (referecen => reference)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L230), [`JBTiered721DelegateStore.sol#L233 (initialise => initialize)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L233), [`JBTiered721DelegateStore.sol#L497 (accross => across)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L497), [`JBTiered721DelegateStore.sol#L597 (benficiary => beneficiary)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L597), [`JBTiered721DelegateStore.sol#L852 (reservd => reserved)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L852), [`JBTiered721DelegateStore.sol#L862 (transfered => transferred)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L862), [`JBTiered721DelegateStore.sol#L947 (initialise => initialze)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L947), [`JBTiered721DelegateStore.sol#L1032 (initialise => initialze)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1032), [`JBTiered721DelegateStore.sol#L1183 (initialise => initialze)`](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1183)

**Description**:
Spelling errors in comments can cause confusion to both users and developers.

**Recommendation**:
Consider checking all misspellings to ensure they are corrected.


## [NAZ-N7] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.


## [NAZ-N8] Floating Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/)

**Description**:
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

**Recommendation**: 
Consider locking the pragma version.


## [NAZ-N9] Older Version Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/jbx-protocol/juice-nft-rewards/tree/f9893b1497098241dd3a664956d8016ff0d0efd0/)

**Description**:
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. 

**Recommendation**:
Consider using the most recent version.
