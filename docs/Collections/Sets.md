---
title: Set
description: PSSv3.0/7.9.5 Sets
---

# Set {#set}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

## Properties
- Unordered.
- Non-randomizable.
- *Element* can be any [*scalar*](../DataTypes/index.md#datatypes_scalar "e.g., `bit`, `int`, `bool`, `enum`, `string`, `float32`, `float64`, `chandle`") or [*aggregate*](../DataTypes/index.md#datatypes_aggregate "e.g., `array`, `list`, `map`, `set`, `struct`") of scalars.
- Each *element* must be unique.
- Can be nested by any collection types (e.g., `array`, `list`, `map` or `set`).

## Declarations
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `chandle` as *data_type*.

Set can be declared by following syntax:<br>
> set&lt;*data_type*&gt; *identifier*

```sv linenums="1"
set<bit [4] > nibbleSet;    //  nibbleSet: {}
set<int     > intSet   ;    //  intSet   : {}
set<bool    > boolSet  ;    //  boolSet  : {}
set<string  > stringSet;    //  stringSet: {}
set<float32 > floatSet ;    //  floatSet : {}
set<eSTR2NUM> enumSet  ;    //  enumSet  : {} (1)
```

1. Assume defined enum type before
```sv linenums="1"
enum eSTR2NUM {
    ZERO, ONE, TWO
};
```

## Initialization Assignment
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* in initialization assignment.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* in initialization assignment.

Set can be assigned at declaration; otherwise, it will be initialized to empty aggregate literal (`{}`).
```sv linenums="1"
set<bit [4] > nibbleSet = {4'b0001, 4'b0010};
set<int     > intSet    = {      1, 2      };
set<bool    > boolSet   = {  false, true   };
set<string  > stringSet = {    "1", "2"    };
set<float32 > floatSet  = {    1.0, 2.0    };
set<eSTR2NUM> enumSet   = {    ONE, TWO    };   // (1)!
```

1. Assume defined enum type before
```sv linenums="1"
enum eSTR2NUM {
    ZERO, ONE, TWO
};
```

## Set Operators
| Operator                                              | Description                                                                                       |
| :---------------------------------------------------- | :------------------------------------------------------------------------------------------------ |
| [`=`](Sets.md#assignment "Assignment operator `=`")   | Create a copy of the `set`-type expression on the RHS and assigns it to the set on the LHS.       |
| [`==`](Sets.md#equality "Equality operator `==`")     | Evaluetes to **true** if both *size*s are equal and have exactly same *element*s.                 |
| [`!=`](Sets.md#inequality "Inequality operator `!=`") | Evaluetes to **true** whether both *size*s are not equal or do not have exactly same *element*s.  |
| [`in`](Sets.md#in "Set membership operator `in`")     | Evaluetes to **true** if *element* on LHS of `in` is exists in the set.                           |
| [`foreach`](Sets.md#foreach "`foreach` statement")    | Iterates over the set's *element*s.                                                               |

## Set Methods
| Method                                                                                            | Description                                       |
| :------------------------------------------------------------------------------------------------ | :------------------------------------------------ |
| [int `size()`](Sets.md#size "function int `size()`")                                              | Returns the number of *element*s in the set.      |
| [`clear()`](Sets.md#clear "function void `clear()`")                                              | Removes all *element*s from the set.              |
| [`delete(data_type element)`](Sets.md#delete "function void `delete(data_type element)`")         | Remove the *element*s from the set.               |
| [`insert(data_type element)`](Sets.md#insert "function void `insert(data_type element)`")         | Adds the *element* to the set.                    |
| [list&lt;data_type&gt; `to_list()`](Sets.md#to_list "function list&lt;data_type&gt; `to_list()`") | Returns all *element*s to a `list`-type.          |

---

## Assignment operator `=` {#assignment}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* for assignment operator `=`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* for assignment operator `=`.

Create a copy of the `set`-type expression on the RHS and assigns it to the set on the LHS.
Same *element*s in the RHS will be merged automatically and appear only once in the set.
```sv linenums="1"
set<int> intSet;

intSet = {1, 2};    //  intSet: {}     -> {1, 2}
intSet = {3, 3, 4}; //  intSet: {1, 2} -> {3, 4} (1)
```

1. Same element will appear only once.

???+ Note
    Operator that modify contents can only be used within `exec` block or native `function`.

## Equality operator `==` {#equality}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluetes to **true** if both *size*s are equal and have exactly same *element*s.
```sv linenums="1"
set<int   > set_0 = { 1 ,  2      };
set<int   > set_1 = { 2 ,  1      };
set<int   > set_2 = { 1 ,  2 ,  3 };
set<string> set_3 = {"1", "2"     };

bit bitVal_0, bitVal_1, bitVal_2, bitVal_3;
if (set_0 == set_0) bitVal_0 = 1;   //  bitVal_0: 0 -> 1 (1)
if (set_0 == set_1) bitVal_1 = 1;   //  bitVal_1: 0 -> 1 (2)
if (set_0 == set_2) bitVal_2 = 1;   //  bitVal_2: 0 -> 0 (3)
if (set_0 == set_3) bitVal_3 = 1;   //  ILLEGAL (4)
```

1. Equalize *size* and all *element*s.
2. Equalize *size* and all *element*s in different order.
3. Inequalize *size*.
4. Different *data_type* are incomparable.

!!! Warning
    Different *data_type* of two sets should **NOT** be compared.

## Inequality operator `!=` {#inequality}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluetes to **true** whether both *size*s are not equal or do not have exactly same *element*s.
```sv linenums="1"
set<int   > set_0 = { 1 ,  2      };
set<int   > set_1 = { 2 ,  1      };
set<int   > set_2 = { 1 ,  2 ,  3 };
set<string> set_3 = {"1", "2"     };

bit bitVal_0, bitVal_1, bitVal_2, bitVal_3;
if (set_0 != set_0) bitVal_0 = 1;   //  bitVal_0: 0 -> 0 (1)
if (set_0 != set_1) bitVal_1 = 1;   //  bitVal_1: 0 -> 0 (2)
if (set_0 != set_2) bitVal_2 = 1;   //  bitVal_2: 0 -> 1 (3)
if (set_0 != set_3) bitVal_3 = 1;   //  bitVal_3: 0 -> 1 (4)
```

1. Equalize *size*, and all *element*s.
2. Equalize *size*, and all *element*s in different order.
3. Inequalize *size*.
4. Different *data_type* are incomparable.

!!! Warning
    Different *data_type* of two sets should **NOT** be compared.

## Set membership operator `in` {#in}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `float32`, `float64`, and `chandle` as *element*.

Evaluetes to **true** if *element* on LHS of `in` is exists in the set.
```sv linenums="1"
set<int> intSet = {1, 2};

bit bitVal_0, bitVal_1, bitVal_2;
if ( 1  in intSet) bitVal_0 = 1;    //  bitVal_0: 0 -> 1
if ( 3  in intSet) bitVal_1 = 1;    //  bitVal_1: 0 -> 0
if ("1" in intSet) bitVal_2 = 1;    //  ILLEGAL (1)
```

1. Different *data_type* between LHS of `in` and the set's *element* on RHS of `in`.

!!! Warning
    *Data_type* of *element* on LHS of `in` should be **SAME** as the set's *element* on RHS of `in`.

## `foreach` statement {#foreach}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Iterates over the set's *element*s.

Look at [Procedural/`foreach`](../Procedural/index.md#foreach) for more information.
```sv linenums="1"
set<int> intSet = {3, 4};
int intVal = 0;

foreach (i : intSet) {
    intVal = i;     //  intVal: 0 -> 3 -> 4 or 0 -> 4 -> 3
}
```

???+ Warning
    For `set`-type, *index variable* of `foreach` statament should **NOT** be used to specify `set` *element*s.
    ```sv linenums="1"
    foreach (intSet[i]) {}  //  ILLEGAL
    ```

---

## function int `size()` {#size}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns the number of *element*s in the set.
```sv linenums="1"
set<int> intSet = {1, 2};

int intVal = intSet.size(); //  intVal: 0 -> 2
```

## function void `clear()` {#clear}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Removes all *element*s from the set.
```sv linenums="1"
set<int> intSet = {1, 2};

intSet.clear(); // intSet: {1, 2} -> {}
```

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function void `delete(data_type element)` {#delete}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Removes the *element*s from the set.
```sv linenums="1"
set<int> intSet = {1, 2};

intSet.delete( 2 ); //  intSet: {1, 2} -> {1}
intSet.delete( 3 ); //  ILLEGAL (1)
intSet.delete("1"); //  ILLEGAL (2)
```

1. The *element* is not exist in the set.
2. *Data-type* of the *element* is not same as the set.

!!! Warning
    The *element* need to deleted should exists in the set.

???+ Note
    Different to `list`-type or `map`-type, `delete(data_type element)` of `set`-type will not return the *element*.

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function void `insert(data_type element)` {#insert}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* of `insert()`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* of `insert()`.

Adds the *element* to the set.
```sv linenums="1"
set<int> intSet = {1, 2};

intSet.insert( 3 ); //  intSet: {1, 2}    -> {1, 2, 3}
intSet.insert( 2 ); //  intSet: {1, 2, 3} -> {1, 2, 3}
intSet.insert("4"); //  ILLEGAL (1)
```

1. *Data-type* of the *element* is not same as the set.

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function list&lt;data_type&gt; `to_list()` {#to_list}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns all *element*s to a `list`-type.
```sv linenums="1"
set<int> intSet = {1, 2};

list<int> intList = intSet.to_list();   //  intList: {} -> {1, 2}
```
