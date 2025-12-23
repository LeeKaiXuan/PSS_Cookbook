---
title: Array
description: PSSv3.0/7.9.2 Arrays
---

# Array {#array}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>

## Properties
- Ordered by *index*.
- Randomizable when its *data_type* is randomizable (e.g., randomizable [*scalar*](../DataTypes/index.md#datatypes_scalar "e.g., `bit`, `int`, `bool`, `enum`, `string`") or [*aggregate*](../DataTypes/index.md#datatypes_aggregate "e.g., `array`, `list`, `struct`") of randomizable scalar).
- *Element* and *data_type* can be any [*scalar*](../DataTypes/index.md#datatypes_scalar "e.g., `bit`, `int`, `bool`, `enum`, `string`, `float32`, `float64`, `chandle`") or [*aggregate*](../DataTypes/index.md#datatypes_aggregate "e.g., `array`, `list`, `map`, `set`, `struct`") of scalar.
- *Index* must be non-negative integer.
- *Size* must be non-zero integer.
- Can be nested by any collection types (e.g., `array`, `list`, `map` or `set`).

## Declarations
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `float32`, `float64`, and `chandle` as *data_type*.

Array has 2 declaration formats:

| Format        | Syntax                                        |
| :------------ | :-------------------------------------------- |
| `Template`    | array&lt;*data_type*, *size*&gt; *identifier* |
| `Square`      | *data_type* *identifier* [*size*]             |

=== "Template"
    ```sv linenums="1"
    array<int    , 3> intArray   ;          //  intArray   : {   0,    0,    0}
    array<bit [8], 3> byteArray  ;          //  byteArray  : {8'b0, 8'b0, 8'b0}
    array<string , 3> stringArray;          //  stringArray: {  "",   "",   ""}
    array<array<int, 3>, 2> nestedArray;    //  nestedArray: {{0, 0, 0}, {0, 0, 0}}
    ```

=== "Square"
    ```sv linenums="1" hl_lines="4"
    int     intArray    [3];                //  intArray   : {   0,    0,    0}
    bit [8] byteArray   [3];                //  byteArray  : {8'b0, 8'b0, 8'b0}
    string  stringArray [3];                //  stringArray: {  "",   "",   ""}
    // (1)!
    ```

    1. `square` format **NOT** support for declare nested array

???+ Note
    Array is a **fixed-size** collection, which's *size* **must** be assigned by a non-zero integer, and cannot be changed after declared.

## Declare array by `rand` keyword
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

=== "Template"
    ```sv linenums="1"
    rand array<int, 3> intArray;    //  declare integer array with 3 random elements
    ```

=== "Square"
    ```sv linenums="1"
    rand int intArray [3];          //  declare integer array with 3 random elements
    ```

## Initialization Assignment
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* in initialization assignment.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* in initialization assignment.

Array can be assigned at declaration; otherwise, each elements will be initialized to default initial value.
=== "Template"
    ```sv linenums="1"
    array<int    , 2> intArray    = {    1,     2};         //  intArray   : {    1,     2}
    array<bit [8], 2> byteArray   = {8'b01, 8'b10};         //  byteArray  : {8'b01, 8'b10}
    array<string , 2> stringArray = {  "A",   "B"};         //  stringArray: {  "A",   "B"}
    array<array<int, 2>, 2> nestedArray = {{1, 2}, {3, 4}}; //  nestedArray: {{1, 2}, {3, 4}}
    ```

=== "Square"
    ```sv linenums="1" hl_lines="4"
    int     intArray    [2] = {    1,     2};               //  intArray   : {    1,     2}
    bit [8] byteArray   [2] = {8'b01, 8'b10};               //  byteArray  : {8'b01, 8'b10}
    string  stringArray [2] = {  "A",   "B"};               //  stringArray: {  "A",   "B"}
    // (1)!
    ```

    1. `square` format **NOT** support for declare nested array

## Array Operators
| Operator  | Description   |
| :-------- | :------------ |
| [`[]`](Arrays.md#index "Index operator `[]`")             | Used to access a specific *element* of an array by given *index*, which must be a non-negative integer.                   |
| [`=`](Arrays.md#assignment "Assignment operator `=`")     | Creates a copy of the `array`-type expression on the RHS and assigns it to the array on the LHS.                      |
| [`==`](Arrays.md#equality "Equality operator `==`")       | Evaluates to **true** if both *size*s are equal and all *element*s with corresponding *index*es are equal.            |
| [`!=`](Arrays.md#inequality "Inequality operator `!=`")   | Evaluates to **true** whether both *size*s are not equal or if any *element* with corresponding *index* is not equal. |
| [`in`](Arrays.md#in "Set membership operator `in`")       | Evaluates to **true** if *element* on LHS of `in` is exists in the array.                                             |
| [`foreach`](Arrays.md#foreach "`foreach` statement")      | Iterates over the array's *element*s.                                                                                 |

## Array Methods
| Method    | Description   |
| :-------- | :------------ |
| [int `size()`](Arrays.md#size "function int `size()`")                                                | Returns the number of *element*s in the array.                                                    |
| [int `sum()`](Arrays.md#sum_int "function int `sum()`")                                               | Returns the sum of all *element*s in the array, when *data_type* of *element* is `bit` or `int`.  |
| [float64 `sum()`](Arrays.md#sum_float "function float64 `sum()`")<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span> <span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span> | Returns the sum of all *element*s in the array, when *data_type* of *element* is `float32` or `float64`.  |
| [list&lt;data_type&gt; `to_list()`](Arrays.md#to_list "function list&lt;data_type&gt; `to_list()`")   | Returns all *element*s to a `list`-type.                                                          |
| [set&lt;data_type&gt; `to_set()`:](Arrays.md#to_set "function set&lt;data_type&gt; `to_set()`")       | Returns all *element*s to a `set`-type.                                                           |

---

## Index operator `[]` {#index}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* for index operator `[]`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* for index operator `[]`.

Used to access a specific *element* of an array by given *index*, which must be a non-negative integer.
=== "Template"
    ```sv linenums="1"
    array<int, 3> intArray = {1, 2, 3};

    int intVal = intArray[2];   //  intVal: 0 -> 3
    ```

=== "Square"
    ```sv linenums="1"
    int intArray [3] = {1, 2, 3};

    int intVal = intArray[2];   //  intVal: 0 -> 3
    ```

## Assignment operator `=` {#assignment}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* for assignment operator `=`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* for assignment operator `=`.

Creates a copy of the `array`-type expression on the RHS and assigns it to the array on the LHS.
=== "Template"
    ```sv linenums="1"
    array<int, 3> intArray = {1, 2, 3};

    intArray = {3, 4, 5};       //  intArray: {1, 2, 3} -> {3, 4, 5}
    ```

=== "Square"
    ```sv linenums="1"
    int intArray [3] = {1, 2, 3};

    intArray = {3, 4, 5};       //  intArray: {1, 2, 3} -> {3, 4, 5}
    ```

???+ Note
    Operator that modify contents can only be used within `exec` block or native `function`.

## Equality operator `==` {#equality}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluates to **true** if both *size*s are equal and all *element*s with corresponding *index*es are equal.
=== "Template"
    ```sv linenums="1"
    array<int   , 2> intArray_0  = {  1 ,  2      };
    array<int   , 3> intArray_1  = {  1 ,  2 ,  3 };
    array<string, 2> stringArray = { "1", "2"     };

    bit bitVal_0, bitVal_1, bitVal_2;
    if (intArray_0 == intArray_0 ) bitVal_0 = 1;    //  bitVal_0: 0 -> 1 (1)
    if (intArray_0 == intArray_1 ) bitVal_1 = 1;    //  bitVal_1: 0 -> 0 (2)
    if (intArray_0 == stringArray) bitVal_2 = 1;    //  ILLEGAL (3)
    ```

    1. Equalize *size* and all *element*s with corresponding *index*es.
    2. Inequalize *size*.
    3. Different *data_type* between arrays are incomparable.

=== "Square"
    ```sv linenums="1"
    int    intArray_0  [2] = {  1 ,  2      };
    int    intArray_1  [3] = {  1 ,  2 ,  3 };
    string stringArray [2] = { "1", "2"     };

    bit bitVal_0, bitVal_1, bitVal_2;
    if (intArray_0 == intArray_0 ) bitVal_0 = 1;    //  bitVal_0: 0 -> 1 (1)
    if (intArray_0 == intArray_1 ) bitVal_1 = 1;    //  bitVal_1: 0 -> 0 (2)
    if (intArray_0 == stringArray) bitVal_2 = 1;    //  ILLEGAL (3)
    ```

    1. Equalize *size* and all *element*s with corresponding *index*es.
    2. Inequalize *size*.
    3. Different *data_type* between arrays are incomparable.

!!! Warning
    Different *data_type* of *element* of two arrays should **NOT** be compared.

## Inequality operator `!=` {#inequality}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluates to **true** whether both *size*s are not equal or if any *element* with corresponding *index* is not equal.
=== "Template"
    ```sv linenums="1"
    array<int   , 2> intArray_0  = {  1 ,  2      };
    array<int   , 3> intArray_1  = {  1 ,  2 ,  3 };
    array<string, 2> stringArray = { "1", "2"     };

    bit bitVal_0, bitVal_1, bitVal_2;
    if (intArray_0 != intArray_0 ) bitVal_0 = 1;    //  bitVal_0: 0 -> 0 (1)
    if (intArray_0 != intArray_1 ) bitVal_1 = 1;    //  bitVal_1: 0 -> 1 (2)
    if (intArray_0 != stringArray) bitVal_2 = 1;    //  ILLEGAL (3)
    ```

    1. Equalize *size* and all *element*s with corresponding *index*es.
    2. Inequalize *size*.
    3. Different *data_type* between arrays are incomparable.

=== "Square"
    ```sv linenums="1"
    int    intArray_0  [2] = {  1 ,  2      };
    int    intArray_1  [3] = {  1 ,  2 ,  3 };
    string stringArray [2] = { "1", "2"     };

    bit bitVal_0, bitVal_1, bitVal_2;
    if (intArray_0 != intArray_0 ) bitVal_0 = 1;    //  bitVal_0: 0 -> 0 (1)
    if (intArray_0 != intArray_1 ) bitVal_1 = 1;    //  bitVal_1: 0 -> 1 (2)
    if (intArray_0 != stringArray) bitVal_2 = 1;    //  ILLEGAL (3)
    ```

    1. Equalize *size* and all *element*s with corresponding *index*es.
    2. Inequalize *size*.
    3. Different *data_type* between arrays are incomparable.

!!! Warning
    Different *data_type* of *element* of two arrays should **NOT** be compared.

## Set membership operator `in` {#in}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluates to **true** if *element* on LHS of `in` is exists in the array.
=== "Template"
    ```sv linenums="1"
    array<int, 3> intArray = {1, 2, 3};

    bit bitVal_0, bitVal_1, bitVal_2;
    if ( 1  in intArray) bitVal_0 = 1;              //  bitval_0: 0 -> 1
    if ( 0  in intArray) bitVal_1 = 1;              //  bitVal_1: 0 -> 0
    if ("1" in intArray) bitVal_2 = 1;              //  ILLEGAL (1)
    ```

    1. Different *data_type* between LHS of `in` and the array's *element* on RHS of `in`.

=== "Square"
    ```sv linenums="1"
    int intArray [3] = {1, 2, 3};

    bit bitVal_0, bitVal_1, bitVal_2;
    if ( 1  in intArray) bitVal_0 = 1;              //  bitval_0: 0 -> 1
    if ( 0  in intArray) bitVal_1 = 1;              //  bitVal_1: 0 -> 0
    if ("1" in intArray) bitVal_2 = 1;              //  ILLEGAL (1)
    ```

    1. Different *data_type* between LHS of `in` and the array's *element* on RHS of `in`.

!!! Warning
    *Data_type* of *element* on LHS of `in` should be **SAME** as the array's *element* on RHS of `in`.

## `foreach` statement {#foreach}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Iterates over the array's *element*s.

Look at [Procedural/`foreach`](../Procedural/index.md#foreach) for more information.
=== "Template"
    ```sv linenums="1"
    array<int, 3> intArray = {4, 5, 6};
    int intVal_0 = 0;
    int intVal_1 = 0;
    int intVal_2 = 0;

    foreach (i : intArray[j]) {
        intVal_0 = i;               //  intVal_0: 0 -> 4 -> 5 -> 6
        intVal_1 = j;               //  intVal_1: 0 -> 0 -> 1 -> 2
        intVal_2 = intArray[j];     //  intVal_2: 0 -> 4 -> 5 -> 6
    }
    ```

=== "Square"
    ```sv linenums="1"
    int intArray [3] = {1, 2, 3};
    int intVal_0 = 0;
    int intVal_1 = 0;
    int intVal_2 = 0;

    foreach (i : intArray[j]) {
        intVal_0 = i;               //  intVal_0: 0 -> 4 -> 5 -> 6
        intVal_1 = j;               //  intVal_1: 0 -> 0 -> 1 -> 2
        intVal_2 = intArray[j];     //  intVal_2: 0 -> 4 -> 5 -> 6
    }
    ```

???+ Tip "Usage: Constraint random array by using `foreach()`"
    For a random array, the `foreach()` method can be used to constraint each *element*'s possible value.

    === ":fontawesome-regular-face-frown:{.red} List *element* one-by-one"
        ```sv linenums="1"
        rand array<bit [4], 4> nibbleArray;
        constraint {
            nibbleArray[0] in [2..3];
            nibbleArray[1] in [2..3];
            nibbleArray[2] in [2..3];
            nibbleArray[3] in [2..3];
        }
        ...
        //  do something with solved nibbleArray
        ```

    === ":fontawesome-regular-face-smile:{.green} Using `foreach`"
        ```sv linenums="1"
        rand array<bit [4], 4> nibbleArray;
        constraint {
            foreach(nibbleArray[i]) nibbleArray[i] in [2..3];   // (1)!
        }
        ...
        //  do something with solved nibbleArray
        ```

        1. Note that, using *iterator_identifier* format (e.g., `foreach(i : nibbleArray)`) can't constraint the array's *elements*.

---

## function int `size()` {#size}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns the number of *element*s in the array.
=== "Template"
    ```sv linenums="1"
    array<int, 2> intArray;

    int intVal = intArray.size();       //  intVal: 0 -> 2
    ```

=== "Square"
    ```sv linenums="1"
    int intArray [2];

    int intVal = intArray.size();       //  intVal: 0 -> 2
    ```

???+ note
    Considered as a **constant** expression.

## function int `sum()` {#sum_int}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns the sum of all *element*s in the array, when *data_type* of *element* is `bit` or `int`.
=== "Template"
    ```sv linenums="1"
    array<int    , 2> intArray    = {  1   ,  2    };
    array<bit [2], 2> bitArray    = { 2'b01, 2'b10 };
    array<string , 2> stringArray = { "1"  , "2"   };

    int intVal_0 = intArray.sum()   ;   //  intVal_0: 0 -> 3
    int intVal_1 = bitArray.sum()   ;   //  intVal_1: 0 -> 3
    int intVal_2 = stringArray.sum();   //  ILLEGAL (1)
    ```

    1. `string` can't be calculated

=== "Square"
    ```sv linenums="1"
    int     intArray    [2] = {  1   ,  2    };
    bit [2] bitArray    [2] = { 2'b01, 2'b10 };
    string  stringArray [2] = { "1"  , "2"   };

    int intVal_0 = intArray.sum()   ;   //  intVal_0: 0 -> 3
    int intVal_1 = bitArray.sum()   ;   //  intVal_1: 0 -> 3
    int intVal_2 = stringArray.sum();   //  ILLEGAL (1)
    ```

    1. `string` can't be calculated

!!! Warning
    The `data-type` of `element` should be integer types (e.g., `int` or `bit`).

???+ tip "Usage: use `sum()` to constrain a random array"
    === "Template"
        ```sv linenums="1"
        rand array<int, 2> intArray;

        constraint {
            intArray.sum() == 3;
            intArray[0] == 2;   //  intArray: {0, 0} -> {2, 1}
        }
        ```

    === "Square"
        ```sv linenums="1"
        rand int intArray [2];

        constraint {
            intArray.sum() == 3;
            intArray[0] == 2;   //  intArray: {0, 0} -> {2, 1}
        }
        ```

## function float64 `sum()` {#sum_float}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span>

Returns the sum of all *element*s in the array, when *data_type* of *element* is `float32` or `float64`.
=== "Template"
    ```sv linenums="1"
    array<float32, 2> float32Array = {1.0, 2.0};
    array<flaot64, 2> float64Array = {1.0, 2.0};
    array<string , 2> stringArray  = {"1", "2"};

    flaot64 fVal_0 = intArray.sum()   ; //  fVal_0: 0.0 -> 3.0
    flaot64 fVal_1 = bitArray.sum()   ; //  fVal_1: 0.0 -> 3.0
    flaot64 fVal_2 = stringArray.sum(); //  ILLEGAL (1)
    ```

    1. `string` can't be calculated

=== "Square"
    ```sv linenums="1"
    float32 float32Array [2] = {1.0, 2.0};
    flaot64 float64Array [2] = {1.0, 2.0};
    string  stringArray  [2] = {"1", "2"};

    flaot64 fVal_0 = intArray.sum()   ; //  fVal_0: 0.0 -> 3.0
    flaot64 fVal_1 = bitArray.sum()   ; //  fVal_1: 0.0 -> 3.0
    flaot64 fVal_2 = stringArray.sum(); //  ILLEGAL (1)
    ```

    1. `string` can't be calculated

!!! Warning
    The `data-type` of `element` should be floating-point types (e.g., `float32` or `float64`).

## function list&lt;data_type&gt; `to_list()` {#to_list}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns all *element*s to a `list`-type.
=== "Template"
    ```sv linenums="1"
    array<int   , 3> intArray    = {  2 ,  1 ,  2  };
    array<string, 3> stringArray = { "2", "1", "2" };

    list<int   > intList    = intArray.to_list()   ;    //  intList: {} -> {2, 1, 2}
    list<string> stringList = stringArray.to_list();    //  stringList: {} -> {"2", "1", "2"};
    ```

=== "Square"
    ```sv linenums="1"
    int    intArray    [3] = {  2 ,  1 ,  2  };
    string stringArray [3] = { "2", "1", "2" };

    list<int   > intList    = intArray.to_list()   ;    //  intList: {} -> {2, 1, 2}
    list<string> stringList = stringArray.to_list();    //  stringList: {} -> {"2", "1", "2"};
    ```

## function set&lt;data_type&gt; `to_set()` {#to_set}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns all *element*s to a `set`-type.
=== "Template"
    ```sv linenums="1"
    array<int   , 3> intArray    = {  2 ,  1 ,  2  };
    array<string, 3> stringArray = { "2", "1", "2" };

    set<int   > intSet    = intArray.to_set()   ;       //  intSet: {} -> {2, 1}
    set<string> stringSet = stringArray.to_set();       //  stringSet: {} -> {"2", "1"}
    ```

=== "Square"
    ```sv linenums="1"
    int    intArray    [3] = {  2 ,  1 ,  2  };
    string stringArray [3] = { "2", "1", "2" };

    set<int   > intSet    = intArray.to_set()   ;       //  intSet: {} -> {2, 1}
    set<string> stringSet = stringArray.to_set();       //  stringSet: {} -> {"2", "1"}
    ```
