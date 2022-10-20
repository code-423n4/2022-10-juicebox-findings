- [Gas](#gas)
    - [**1. Don't use the length of an array for loops condition**](#1-dont-use-the-length-of-an-array-for-loops-condition)
    - [**2. Avoid compound assignment operator in state variables**](#2-avoid-compound-assignment-operator-in-state-variables)
        - [Total gas saved: **13 * 1 = 13**](#total-gas-saved-13--1--13)
    - [**3. Use string.concat instead of abi.encodePacked**](#3-use-stringconcat-instead-of-abiencodepacked)
    - [**4. Use calldata instead of memory**](#4-use-calldata-instead-of-memory)
    - [**5. ++i costs less gas compared to i++ or i += 1**](#5-i-costs-less-gas-compared-to-i-or-i--1)
    - [**6. Use Custom Errors instead of Revert Strings to save Gas**](#6-use-custom-errors-instead-of-revert-strings-to-save-gas)
        - [Total gas saved: **9 * 2 = 18**](#total-gas-saved-9--2--18)
    - [**7. delete optimization**](#7-delete-optimization)
        - [Total gas saved: **5 * 4 = 20**](#total-gas-saved-5--4--20)
    - [**8. There's no need to set default values for variables**](#8-theres-no-need-to-set-default-values-for-variables)
        - [Total gas saved: **8 * 1 = 9**](#total-gas-saved-8--1--9)
    - [**9. Remove natspec complaints**](#9-remove-natspec-complaints)
    - [**10. Use the unchecked keyword**](#10-use-the-unchecked-keyword)

# Gas

## **1. Don't use the length of an array for loops condition**

It's cheaper to store the length of the array inside a local variable and iterate over it.

**Affected source code:**

- [JBIpfsDecoder.sol:49](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49)
- [JBIpfsDecoder.sol:76](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)
- [JBIpfsDecoder.sol:84](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)

## **2. Avoid compound assignment operator in state variables**

Using compound assignment operators for state variables (like `State += X` or `State -= X` ...) it's more expensive than using operator assignment (like `State = State + X` or `State = State - X` ...).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
uint private _a;
function testShort() public {
_a += 1;
}
}

contract TesterB {
uint private _a;
function testLong() public {
_a = _a + 1;
}
}
```

Gas saving executing: **13 per entry**

```
TesterA.testShort: 43507
TesterB.testLong:  43494
```

**Affected source code:**

- [JBTiered721DelegateStore.sol:827](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L827)

### Total gas saved: **13 * 1 = 13**

## **3. Use `string.concat` instead of `abi.encodePacked`**

`abi.encodePacked` has a cost difference with respect to `string.concat` depending on the types it uses. In the case of the reference shown below you can see that the type `address` is affected, so I have decided to do a test on it.

Also there is a discussion about removing `abi.encodePacked` from future versions of Solidity ([ethereum/solidity#11593](https://github.com/ethereum/solidity/issues/11593)), so using `abi.encode` now will ensure compatibility in the future.

**Affected source code:**

- [JBIpfsDecoder.sol:28](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L28)
- [JBIpfsDecoder.sol:34](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L34)

## **4. Use `calldata` instead of `memory`**

Some methods are declared as `external` but the arguments are defined as `memory` instead of as `calldata`.

By marking the function as `external` it is possible to use `calldata` in the arguments shown below and save significant gas.

**Recommended change:**

```diff
- function mintReservesFor(JBTiered721MintReservesForTiersData[] memory _mintReservesForTiersData)
+ function mintReservesFor(JBTiered721MintReservesForTiersData[] calldata _mintReservesForTiersData)
    external
    override
  {
  ...
  }
```

**Affected source code:**

- [JBTiered721Delegate.sol:264-282](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L264-L282)
- [JBTiered721Delegate.sol:290-309](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L290-L309)
- [JBTiered721Delegate.sol:386-391](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L386-L391)
- [JBTiered721DelegateStore.sol:628-794](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L628-L794)
- [JBTiered721DelegateStore.sol:1091-1114](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1091-L1114)

## **5. `++i` costs less gas compared to `i++` or `i += 1`**

`++i` costs less gas compared to `i++` or `i += 1` for unsigned integers, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.

`i++` increments `i` and returns the initial value of `i`. Which means:

```solidity
uint i = 1;
i++; // == 1 but i == 2
```

But `++i` returns the actual incremented value:

```solidity
uint i = 1;
++i; // == 2 and i == 2 too, so no need for a temporary variable
```

In the first case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`
I suggest using `++i` instead of `i++` to increment the value of an uint variable. Same thing for `--i` and `i--`

*Keep in mind that this change can only be made when we are not interested in the value returned by the operation, since the result is different, you only have to apply it when you only want to increase a counter.*

**Affected source code:**

- [JBTiered721DelegateStore.sol:1106](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1106)
- [JBTiered721DelegateStore.sol:1108](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1108)
- [JBIpfsDecoder.sol:59](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L59)
- [JBIpfsDecoder.sol:68](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68)
- [JBIpfsDecoder.sol:76](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L76)
- [JBIpfsDecoder.sol:84](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L84)

## **6. Use Custom Errors instead of Revert Strings to save Gas**

Custom errors from Solidity `0.8.4` are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met)

**Source Custom Errors in Solidity:**

Starting from Solidity `v0.8.4`, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
function testRevert(bool path) public view {
 require(path, "test error");
}
}

contract TesterB {
error MyError(string msg);
function testError(bool path) public view {
 if(path) revert MyError("test error");
}
}
```

Gas saving executing: **9 per entry**

```
TesterA.testRevert: 21611
TesterB.testError:  21602     
```

**Affected source code:**

- [JBTiered721Delegate.sol:216](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L216)
- [JBTiered721Delegate.sol:218](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L218)

### Total gas saved: **9 * 2 = 18**

## **7. `delete` optimization**

Use `delete` instead of set to default value (`false` or `0`).

5 gas could be saved per entry in the following affected lines:

**Affected source code:**

- [JBTiered721Delegate.sol:588](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L588)
- [JBTiered721Delegate.sol:589](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol#L589)
- [JBTiered721DelegateStore.sol:772](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L772)
- [JBTiered721DelegateStore.sol:1198](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol#L1198)

### Total gas saved: **5 * 4 = 20**

## **8. There's no need to set default values for variables**

If a variable is not set/initialized, the default value is assumed (0, `false`, 0x0 ... depending on the data type). You are simply wasting gas if you directly initialize it with its default value.

**Proof of concept (*without optimizations*):**

```javascript
pragma solidity 0.8.15;

contract TesterA {
function testInit() public view returns (uint) { uint a = 0; return a; }
}

contract TesterB {
function testNoInit() public view returns (uint) { uint a; return a; }
}
```

Gas saving executing: **8 per entry**

```
TesterA.testInit:   21392
TesterB.testNoInit: 21384
```

**Affected source code:**

- [JBIpfsDecoder.sol:47](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L47)

### Total gas saved: **8 * 1 = 9**

## **9. Remove natspec complaints**

Remove natspec complaints in order to save gas. This is not the best way to avoid some warnings.

**Affected source code:**

- [JB721GlobalGovernance.sol:57](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721GlobalGovernance.sol#L57)
- [JB721TieredGovernance.sol:315](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol#L315)

## **10. Use the `unchecked` keyword**

When an underflow or overflow cannot occur, one might conserve gas by using the `unchecked` keyword to prevent unnecessary arithmetic underflow/overflow tests.

**Affected source code:**

- [JBIpfsDecoder.sol:49](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L49)
- [JBIpfsDecoder.sol:51](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L51)
- [JBIpfsDecoder.sol:68](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L68)
- [JBIpfsDecoder.sol:77](https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol#L77)
