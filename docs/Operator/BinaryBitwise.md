---
title: Binary Bitwise Operators
description: PSSv3.0/8.5.5 Bitwise operators
---

# Binary Bitwise Operators {#operator_binarybiwise_binary_bitwise_operators}
PSS supports 3 basic bitwise operators:
1. Bitwise AND `&`
2. Bitwise OR `|`
3. Bitwise XOR `^`

## CONSTANT **op** CONSTANT
```sv linenums="1"
bit [8] byteAND = 8'b0110_1001 & 8'b0000_1111;  //  byteAND: 0 -> 8'b0000_1001
bit [8] byteOR  = 8'b0110_1001 | 8'b0000_1111;  //  byteOR : 0 -> 8'b0110_1111
bit [8] byteXOR = 8'b0110_1001 ^ 8'b0000_1111;  //  byteXOR: 0 -> 8'b0110_0110
```
