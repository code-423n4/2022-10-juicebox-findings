# use of child contract were base contract would be more appropriate
In JBTiered721DelegateDeployer.sol L. 83-84 the `initialize` function is called on a contract instance which can be of type `JB721GlobalGovernance`, `JB721TieredGovernance` or `JB721TieredDelegate`:
```js
JB721GlobalGovernance newDelegate = JB721GlobalGovernance(_clone(codeToCopy));
newDelegate.initialize(
    ...
```
Even though this works fine, due to the way inter-contract calls work in the EVM (would break in other statically typed languages), for clarity of code this should be casted to the common base contract which defines the `initialize` function, namely `JB721TieredDelegate`:
```js
JB721TieredDelegate newDelegate = JB721TieredDelegate(_clone(codeToCopy));
newDelegate.initialize(
    ...
```

# unbounded loop which could affect GlobalGovernance
The function `JBTiered721DelegateStore.votingUnitsOf` iterates over all tiers defined on a given NFT-contract. Hypothetically this could cause calls of the function to run out of gas for a large amount of tiers and break `JB721GlobalGovernance._getVotingUnits` which is again used by `Votes._delegate`. In practice this seems unlikely to happen, as the function does not perform heavy calculations and would require a very large number of tiers for this to happen.