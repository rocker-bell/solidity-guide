# solidity-guide

## Motivation

This document is a cheatsheet for **Solidity** that you can use to write **Smart Contracts** for **Ethereum** based blockchain.

This guide is not intended to teach you Solidity from the ground up, but to help developers with basic knowledge who may struggle to get familiar with Smart Contracts and Blockchain because of the Solidity concepts used.

> **Note:** If you have basic knowledge in JavaScript, it's easier to learn Solidity.

---

## Version pragma

```solidity
pragma solidity ^0.8.0;
```

`pragma solidity ^0.8.0;` compiles with compiler version >= 0.8.0 and < 0.9.0.

---

## Import files

```solidity
import "filename.sol";
import * as symbolName from "filename.sol";
import {symbol1 as alias, symbol2} from "filename.sol";
```

---

## Types

### Boolean

`bool` : `true` or `false`

Operators:
- Logical : `!`, `&&`, `||`
- Comparisons : `==`, `!=`

---

### Integer

Unsigned : `uint8 | uint16 | uint32 | uint64 | uint128 | uint256 (uint)`
Signed   : `int8  | int16  | int32  | int64  | int128  | int256 (int)`

Operators:
- Comparisons: `<=`, `<`, `==`, `!=`, `>=`, `>`
- Bit operators: `&`, `|`, `^`, `~`
- Arithmetic operators: `+`, `-`, `*`, `/`, `%`, `**`, `<<`, `>>`

> In Solidity 0.8.x, arithmetic overflow and underflow automatically revert.
> SafeMath is no longer required.

---

### Address

`address`: Holds an Ethereum address (20 bytes).  
`address payable`: Same as address but allows `transfer` and `send`.

Methods:

#### balance
```solidity
address(this).balance;
```

#### transfer and send
```solidity
payable(addr).transfer(amount);   // reverts on failure
payable(addr).send(amount);       // returns false on failure
```

#### call
```solidity
(bool success, bytes memory data) = addr.call{value: 1 ether}("");
require(success);
```

#### delegatecall
```solidity
(bool success, ) = addr.delegatecall(
    abi.encodeWithSignature("setValue(uint256)", 10)
);
require(success);
```

> delegatecall executes code of another contract in the context of the calling contract.

---

### Array

```solidity
uint[] dynamicArray;
uint[7] fixedArray;
```

---

### Fixed byte arrays

`bytes1` ... `bytes32`

Members:
- `.length` (read-only)

---

### Dynamic byte arrays

`bytes` – dynamic byte array  
`string` – UTF-8 encoded string

---

### Enum

```solidity
enum ActionChoices { GoLeft, GoRight, GoStraight, SitStill }

ActionChoices choice = ActionChoices.GoStraight;
```

---

### Struct

```solidity
struct Funder {
    address addr;
    uint amount;
}
```

---

### Mapping

```solidity
mapping(address => uint) public balances;
```

Mappings behave like hash tables.

---

## Control Structures

Available:
- `if / else`
- `while`
- `do while`
- `for`
- `break`
- `continue`
- `return`
- `? :`

---

## Functions

### Structure

```solidity
function name(parameters)
    public | private | internal | external
    pure | view | payable
    returns (type)
```

---

### Access Modifiers

- `public`
- `private`
- `internal`
- `external`

---

### Constructor

```solidity
contract C {
    address owner;

    constructor() {
        owner = msg.sender;
    }
}
```

---

### External Call Syntax (0.8.x)

```solidity
contractInstance.functionName{value: 1 ether, gas: 50000}();
```

---

### View Functions

```solidity
function getValue() public view returns (uint) {
    return 5;
}
```

---

### Pure Functions

```solidity
function multiply(uint a) public pure returns (uint) {
    return a * 42;
}
```

---

### Payable Functions

```solidity
function deposit() public payable {}
```

---

### Fallback & Receive (0.8.x)

```solidity
receive() external payable {}

fallback() external payable {}
```

---

## Contracts

### Creating contracts using `new`

```solidity
contract A {}

contract C {
    A a = new A();
}
```

---

### Inheritance

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

## Interface

```solidity
interface Token {
    function transfer(address recipient, uint amount) external returns (bool);
}
```

---

## Events

```solidity
event Deposit(address indexed from, uint amount);

function deposit() public payable {
    emit Deposit(msg.sender, msg.value);
}
```

---

## Library

```solidity
library Math {
    function add(uint a, uint b) internal pure returns (uint) {
        return a + b;
    }
}
```

---

## Using For

```solidity
using Math for uint;

uint result = 5.add(3);
```

---

## Error Handling

```solidity
require(condition, "Error message");
assert(condition);
revert("Error message");
```

### Custom Errors (0.8.4+)

```solidity
error NotOwner();

if (msg.sender != owner) revert NotOwner();
```

---

## Global Variables

### Block
- `block.number`
- `block.timestamp`
- `block.coinbase`
- `block.gaslimit`
- `block.difficulty`

### Transaction
- `msg.sender`
- `msg.value`
- `msg.data`
- `msg.sig`
- `tx.origin`
- `tx.gasprice`

---

## Cryptographic Functions

- `keccak256()`
- `sha256()`
- `ripemd160()`
- `ecrecover()`
- `addmod()`
- `mulmod()`

---

## Contract Related

- `this`
- `selfdestruct(address payable recipient)`
