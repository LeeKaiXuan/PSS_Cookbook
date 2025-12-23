---
title: Primary Expressions
description: PSSv3.0/8.6 Primary expressions
---

# Primary Expressions

## Bit-selects {#primary_expressions_bit-selects}
<span class="mdx-badge">
<span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span>
</span>

Used to select a particular bit from integer variable or constant by specified an *index*.

Get a bit of variable:
```sv linenums="1"
bit [4] nibbleVal = 4'b0011;

bit bitVal = nibbleVal[1];      //  bitVal: 0 -> 1
```

Assign a bit of variable:
```sv linenums="1"
bit [4] nibbleVal = 4'b0011;

nibbleVal[1] = 0;               //  nibbleVal: 0b0011 -> 0b0001
```

Assign a bit of variable by a non-zero integer:
```sv linenums="1"
bit [4] nibbleVal = 4'b0011;

nibbleVal[2] = 2'b10;           //  nibbleVal: 0b0011 -> 0b0111
```

Operate a bit of variable by another bit:
```sv linenums="1"
bit [4] nibbleVal = 4'b0011;

nibbleVal[2] |= nibbleVal[1];   //  nibbleVal: 0b0011 -> 0b0111
```

???+ failure "Illegal: Access out-of-bounds"
    ```sv linenums="1"
    bit [4] nibbleVal = 4'b0011;

    bit bitVal = nibbleVal[10];     //  ILLEGAL (1)
    nibbleVal[10] = 1;              //  ILLEGAL (2)
    nibbleVal[-1] = 1;              //  ILLEGAL SYNTAX (3)
    ```

    1. The *index* is larger than bit-width of variable.
    2. The *index* is larger than bit-width of variable.
    3. The *index* should **NOT** be negative.

!!! Warning
    The *index* should be a non-negative integer and should **NOT** access out-of-bounds.

---

## Part-selects {#primary_expressions_part-selects}
<span class="mdx-badge">
<span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.4.0</span>
</span>

Used to select a fixed range of contiguous bits of variable by specified dual-bounds *index*.

Get 2 bits of variable:
```sv linenums="1"
bit [4] nibbleVal = 4'b0011;

bit [2] bitVal = nibbleVal[1:0];    //  bitVal: 0b00 -> 0b11
bit [2] bitVal = nibbleVal[0:1];    //  ILLEGAL SYNTAX (1)
```

1. LSB of *index* shall be smaller than MSB.

Assign 2 bits of variable:
```sv linenums="1"
bit [4] nibbleVal = 4'b0011;

nibbleVal[1:0] = 2'b00;             //  nibbleVal: 0b0011 -> 0b0000
```

Operate 2 bits of variable by another 2 bits:
```sv linenums="1"
bit [4] nibbleVal = 4'b0011;

nibbleVal[3:2] |= nibbleVal[1:0];   //  nibbleVal: 0b0011 -> 0b1111
```

???+ failure "Illegal: Access out-of-bounds"
    ```sv linenums="1"
    bit [4] nibbleVal = 4'b0011;

    bit [2] bitVal = nibbleVal[4:3];    //  ILLEGAL (1)
    nibbleVal[4:3] = 2'b01;             //  ILLEGAL (2)
    nibbleVal[0:-1] = 2'b01;            //  ILLEGAL SYNTAX (3)
    ```

    1. MSB of *index* is larger than bit-width of variable.
    2. MSB of *index* is larger than bit-width of variable.
    3. LSB of *index* should **NOT** be negative.

!!! Warning
    The *index*es should be a non-negative integer and should **NOT** access out-of-bounds.<br>
    The MSB of *index* should larger than LSB of *index*.

---

## Selecting an element from a collection (indexing) {#primary_expressions_indexing}
<span class="mdx-badge">
<span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span>
</span>

Used as index operator `[]` of `array`, `list`, and `map` collections.

Get an element of collection (e.g., `array`, `list`, `map`):
```sv linenums="1"
array<bit [4], 2>      nibbleArray = {       4'b1100, 4'b0110};
list <bit [4]>         nibbleList  = {       4'b1100, 4'b0110};
map  <string, bit [4]> nibbleMap   = {"ONE": 4'b1100         };

bit [4] nibbleArrayVal = nibbleArray[0]  ;  //  nibbleArrayVal: 0 -> 0b1100
bit [4] nibbleListVal  = nibbleList[1]   ;  //  nibbleListVal : 0 -> 0b0110
bit [4] nibbleMapVal   = nibbleMap["ONE"];  //  nibbleMapVal  : 0 -> 0b1100
```

Assign an element of collection (e.g., array, list, map):
```sv linenums="1"
array<bit [4], 2>      nibbleArray = {       4'b1100, 4'b0110};
list <bit [4]>         nibbleList  = {       4'b1100, 4'b0110};
map  <string, bit [4]> nibbleMap   = {"ONE": 4'b1100         };

nibbleArray[0]   = 4'b0011;  //  nibbleArray[0]  : 0b1100 -> 0b0011
nibbleList[0]    = 4'b0011;  //  nibbleList[0]   : 0b1100 -> 0b0011
nibbleMap["ONE"] = 4'b0001;  //  nibbleMap["ONE"]: 0b1100 -> 0b0001
nibbleMap["TWO"] = 4'b0110;  //  nibbleMap["TWO"]: {} -> 0b0110 (1)
```

1. `map` will add the element if no existed.

Operate an element of collection (e.g., array, list, map):
```sv linenums="1"
array<bit [4], 2>      nibbleArray = {       4'b1100,        4'b0110};
list <bit [4]>         nibbleList  = {       4'b1100,        4'b0110};
map  <string, bit [4]> nibbleMap   = {"ONE": 4'b1100, "TWO": 4'b0110};

nibbleArray[0]   |= nibbleArray[1]  ;  //  nibbleArray[0]  : 0b1100 -> 0b1110
nibbleList[0]    |= nibbleList[1]   ;  //  nibbleList[0]   : 0b1100 -> 0b1110
nibbleMap["ONE"] |= nibbleMap["TWO"];  //  nibbleMap["ONE"]: 0b1100 -> 0b1110
```

???+ failure "Illegal: Access out-of-bounds"
    ```sv linenums="1"
    array<bit [4], 2>      nibbleArray = {       4'b1100, 4'b0110};
    list <bit [4]>         nibbleList  = {       4'b1100, 4'b0110};
    map  <string, bit [4]> nibbleMap   = {"ONE": 4'b1100         };

    bit [4] nibbleArrayVal = nibbleArray[2]  ;  //  ILLEGAL (1)
    bit [4] nibbleListVal  = nibbleList[2]   ;  //  ILLEGAL (2)
    bit [4] nibbleMapVal   = nibbleMap["TWO"];  //  ILLEGAL (3)
    ```

    1. The *index* must be inbound of size.
    2. The *index* must be inbound of size.
    3. The element must be exist in the `map`.

---

## Usage

???+ Example "Access bit of an array"
    ```sv linenums="1"
    array<bit [4], 2> nibbleArray = {4'b1100, 4'b0110};

    bit bitOnArray_0_2 = nibbleArray[0][2]; //  bitOnArray_0_2: 0 -> 1
    nibbleArray[1][0] = 1;                  //  nibbleArray[1]: 0b0110 -> 0b0111
    ```
