---
layout: home
title: Solidity Guide
nav_order: 1
---

# Solidity Guide

A practical cheatsheet for writing Smart Contracts in **Solidity 0.8.x**.

This guide is not meant to teach Solidity from scratch.  
It is intended for developers who already understand programming fundamentals and want a structured reference.

> 💡 If you know JavaScript, learning Solidity is significantly easier.

---

# Version Pragma

```solidity
pragma solidity ^0.8.0;
```

`^0.8.0` compiles with:
>= 0.8.0 and < 0.9.0

> ⚠️ Solidity 0.8.x introduced automatic overflow & underflow protection.

---

# Imports

```solidity
import "filename.sol";
import * as Math from "./Math.sol";
import {symbol1 as alias, symbol2} from "./file.sol";
```

---

# Types

## Boolean

```solidity
bool isActive = true;
```

Operators:
- `!`
- `&&`
- `||`
- `==`
- `!=`

---

## Integers

Unsigned:
```solidity
uint256 value;
uint8 small;
```

Signed:
```solidity
int256 number;
```

Operators:
- Arithmetic: `+ - * / % **`
- Comparison: `<= < > >= == !=`
- Bitwise: `& | ^ ~ << >>`

> ✅ In Solidity 0.8+, overflow automatically reverts.  
> SafeMath is no longer required.

If you intentionally want wrapping behavior:

```solidity
unchecked {
    counter++;
}
```

---

## Address

```solidity
address user;
address payable receiver;
```

### Balance

```solidity
user.balance;
address(this).balance;
```

### Transfer Ether

```solidity
receiver.transfer(1 ether); // reverts on failure
receiver.send(1 ether);     // returns false on failure
```

### Low-Level Call

```solidity
(bool success, bytes memory data) =
    user.call{value: 1 ether}("");

require(success);
```

### delegatecall

```solidity
(bool success, ) = target.delegatecall(
    abi.encodeWithSignature("setValue(uint256)", 10)
);
require(success);
```

> ⚠️ delegatecall executes in the caller’s storage context.

---

## Arrays

```solidity
uint[] dynamicArray;
uint[5] fixedArray;
```

---

## Bytes

Fixed:
```solidity
bytes32 data;
```

Dynamic:
```solidity
bytes memory raw;
string memory text;
```

---

## Enum

```solidity
enum Status { Pending, Approved, Rejected }

Status public current = Status.Pending;
```

---

## Struct

```solidity
struct User {
    address account;
    uint256 balance;
}
```

---

## Mapping

```solidity
mapping(address => uint256) public balances;
```

> Mappings behave like hash tables.

---

# Control Structures

Available:
- `if / else`
- `for`
- `while`
- `do while`
- `break`
- `continue`
- `return`
- ternary `? :`

---

# Functions

## Structure

```solidity
function name(uint a)
    public
    view
    returns (uint)
```

Modifiers:
- `public`
- `private`
- `internal`
- `external`
- `view`
- `pure`
- `payable`

---

## Constructor

```solidity
constructor() {
    owner = msg.sender;
}
```

---

## External Calls (0.8.x Syntax)

```solidity
contractInstance.functionName{value: 1 ether, gas: 50000}();
```

---

## View Function

```solidity
function getBalance() public view returns (uint) {
    return address(this).balance;
}
```

---

## Pure Function

```solidity
function multiply(uint a) public pure returns (uint) {
    return a * 2;
}
```

---

## Payable

```solidity
function deposit() public payable {}
```

---

## Fallback & Receive

```solidity
receive() external payable {}

fallback() external payable {}
```

- `receive()` → triggered when ETH is sent without calldata
- `fallback()` → triggered when function does not exist

---

# Contracts

## Creating with `new`

```solidity
ContractA a = new ContractA();
```

---

## Inheritance

```solidity
contract Owned {
    address public owner;

    constructor() {
        owner = msg.sender;
    }
}

contract Child is Owned {}
```

---

# Interface

```solidity
interface IToken {
    function transfer(address to, uint amount) external returns (bool);
}
```

---

# Events

```solidity
event Deposit(address indexed from, uint amount);

function deposit() public payable {
    emit Deposit(msg.sender, msg.value);
}
```

Up to 3 parameters can be `indexed`.

---

# Library

```solidity
library Math {
    function add(uint a, uint b) internal pure returns (uint) {
        return a + b;
    }
}
```

---

# Using For

```solidity
using Math for uint;

uint result = 5.add(3);
```

---

# Error Handling

```solidity
require(condition, "Error message");
assert(condition);
revert("Error message");
```

## Custom Errors (Gas Efficient)

```solidity
error NotOwner();

if (msg.sender != owner) revert NotOwner();
```

---

# Global Variables

## Block

- `block.number`
- `block.timestamp`
- `block.coinbase`
- `block.gaslimit`
- `block.difficulty`

## Transaction

- `msg.sender`
- `msg.value`
- `msg.data`
- `msg.sig`
- `tx.origin`
- `tx.gasprice`

---

# Cryptographic Functions

- `keccak256()`
- `sha256()`
- `ripemd160()`
- `ecrecover()`
- `addmod()`
- `mulmod()`

---

# Contract Related

- `this`
- `selfdestruct(address payable recipient)`

> ⚠️ selfdestruct is discouraged in modern contract architecture.
