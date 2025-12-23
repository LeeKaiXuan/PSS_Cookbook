---
title: Operators and Expressions
description: PSSv3.0/8 Operators and expressions
---

# Operators and Expressions

## Assignment Operators
*WIP*

## Expression operators and data types
| Operator token                                                                            | Operator name                         | Operator data types                   | Result data type              |
| :---------------------------------------------------------------------------------------- | :------------------------------------ | :-----------------------------------: | :---------------------------: |
| `?:`                                                                                      | Conditional operator                  | Any plain-data type or reference type | Same as operands              |
| `-`                                                                                       | Unary arithmetic negation operator    | Numeric                               | Same as operand               |
| `~`                                                                                       | Unary bitwise negation operator       | Numeric                               | Same as operand               |
| `!`                                                                                       | Unary Boolean negation operator       | Boolean                               | Boolean                       |
| `& | ^`                                                                                   | Unary bitwise reduction operators     | Numeric                               | 1-bit                         |
| `+ - * / % **`                                                                            | Binary arithmetic operators           | Numeric                               | 1-bit                         |
| [`& | ^`](BinaryBitwise.md#operator_binarybiwise_binary_bitwise_operators "AND OR XOR")   | Binary bitwise operators              | Numeric                               | 1-bit                         |
| `>> <<`                                                                                   | Binary shift operators                | Numeric                               | Same as left operand          |
| `&& ||`                                                                                   | Binary Boolean logical operators      | Boolean                               | Same as operands              |
| `< <= > >=`                                                                               | Binary relational operators           | Numeric                               | Boolean                       |
| `== !=`                                                                                   | Binary logical equality operators     | Any plain-data type or reference type | Boolean                       |
| `cast`                                                                                    | Data type conversion operator         | Numeric, Boolean, enum                | Casting type                  |
| `in`                                                                                      | Binary set membership operator        | Any plain-data type                   | Boolean                       |
| `[expression]`                                                                            | Index operator                        | Array, list, map                      | Same as element of collection |
| `[expression]`                                                                            | Bit-select operators                  | Numeric                               | Numeric                       |
| `[expression:expression]`                                                                 | Part-select operator                  | Numeric                               | Numeric                       |

???+ Tip "Usage: avoid overflow when left-shifting"
    Shift operators act on the bit-width of the left operand. If the shift amount on the RHS is greater than or equal to that width, the result overflows to `0`, which is usually meaningless.

    ```sv linenums="1"
    bit [8] byteVal  = 8'h01;
    bit [8] byteShift = byteVal << 9; //  byteShift: 0 -> 8'h00 (overflowed to zero)
    ```

    === ":fontawesome-regular-face-smile:{.green} Solution 1"
        Declare the operand wide enough so the shift amount stays within its bit-width.
        ```sv linenums="1"
        bit [16] wideVal   = 16'h0001;
        bit [16] wideShift = wideVal << 9; //  wideShift: 0 -> 16'h0200 (fits without overflow)
        ```

    === ":fontawesome-regular-face-smile:{.green} Solution 2"
        Assign the narrow value into a wider destination before shifting.
        ```sv linenums="1"
        bit [8 ] narrowVal = 8'h01;
        bit [16] dstVal    = narrowVal;   //  extend to 16 bits first
        dstVal = dstVal << 9;             //  dstVal: 0 -> 16'h0200
        ```

    === ":fontawesome-regular-face-smile:{.green} Solution 3"
        Cast the operand to a wider type before applying the shift.
        ```sv linenums="1"
        bit [8]  byteVal   = 8'h01;
        bit [16] castShift = (cast(bit [16]) byteVal) << 9; //  castShift: 0 -> 16'h0200
        ```

## Operator precedence and associativity
| Operator                          | Associativity | Precesence    |
| :-------------------------------- | :-----------: | :-----------: |
| `()` `[]`                         | Left          | 1 (Highest)   |
| `cast`                            | Right         | 2             |
| `-` `!` `~` `&` `|` `^` (unary)   |               | 2             |
| `**`                              | Left          | 3             |
| `*` `/` `%`                       | Left          | 4             |
| `+` `-` (binary)                  | Left          | 5             |
| `<<` `>>`                         | Left          | 6             |
| `<` `<=` `>` `>=` `in`            | Left          | 7             |
| `==` `!=`                         | Left          | 8             |
| `&` (binary)                      | Left          | 9             |
| `^` (binary)                      | Left          | 10            |
| `|` (binary)                      | Left          | 11            |
| `&&`                              | Left          | 12            |
| `||`                              | Left          | 13            |
| `?:` (conditional operator)       | Right         | 14 (Lowest)   |
