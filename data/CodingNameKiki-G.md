1. Storage variables can be set as immutable - 4k gas saved

Consider declaring values of storage variables as immutable, if the value set inside the constructor isn't modified afterwards.

https://gist.github.com/CodingNameKiki/5dc57dce435c7c039983639a918056c7

2. Using calldata instead of memory for read-only arguments in external functions

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memoryindex. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. Structs have the same overhead as an array of length one

When arguments are read-only on external functions, the data location should be calldata:

https://gist.github.com/CodingNameKiki/16260e7a1bafc96b13b6b5d0410ea98d