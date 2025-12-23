---
title: List
description: PSSv3.0/7.9.3 Lists
---

# List {#list}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

## Properties
- Ordered by *index*.
- Randomizable when its *data_type* is randomizable (e.g., randomizable [*scalar*](../DataTypes/index.md#datatypes_scalar "e.g., `bit`, `int`, `bool`, `enum`, `string`") or [*aggregate*](../DataTypes/index.md#datatypes_aggregate "e.g., `array`, `list`, `struct`") of randomizable scalar).
- *Element* and *data_type* can be any [*scalar*](../DataTypes/index.md#datatypes_scalar "e.g., `bit`, `int`, `bool`, `enum`, `string`, `float32`, `float64`, `chandle`") or [*aggregate*](../DataTypes/index.md#datatypes_aggregate "e.g., `array`, `list`, `map`, `set`, `struct`") of scalar.
- *Index* must be non-negative integer.
- Can be nested by any collection types (e.g., `array`, `list`, `map` or `set`).

## Declarations
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `chandle` as *data_type*.

List can be declared by following syntax:<br>
> list&lt;*data_type*&gt; *identifier*

```sv linenums="1"
list<int     > intList   ;  //  intList   : {}
list<bit [8] > byteList  ;  //  byteList  : {}
list<bool    > boolList  ;  //  boolList  : {}
list<string  > stringList;  //  stringList: {}
list<eSTR2NUM> enumList  ;  //  enumList  : {} (1)
list<sSTR2NUM> structList;  //  structList: {} (2)
```

1. Assume defined enum type before
```sv linenums="1"
enum eSTR2NUM {
    ZERO, ONE, TWO
};
```

2. Assume defined struct type before
```sv linenums="1"
struct sSTR2NUM {
    string stringVal;
    int intVal;
};
```

## Declare list by `rand` keyword
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span>

```sv linenums="1"
rand list<int    > intList   ;
rand list<bit [8]> byteList  ;
rand list<string > stringList;
```

## Initialization Assignment
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* in initialization assignment.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* in initialization assignment.

List can be assigned at declaration; otherwise, it will be initialized to empty aggregate literal (`{}`).
```sv linenums="1"
list<int    > intList    = {1    , 2    };  //  intList   : {1    , 2    }
list<bit [8]> byteList   = {8'b01, 8'b10};  //  byteList  : {8'b01, 8'b10}
list<bool   > boolList   = {false, true };  //  boolList  : {false, true }
list<string > stringList = {"1"  , "2"  };  //  stringList: {"1"  , "2"  }
```

## List Operators
| Operator  | Description   |
| :-------- | :------------ |
| [`[]`](Lists.md#index "Index operator `[]`")              | Used to access a specific *element* of a list by given *index*, which must be a non-negative integer.                 |
| [`=`](Lists.md#assignment "Assignment operator `=`")      | Create a copy of the `list`-type expression on the RHS and assigns it to the list on the LHS.                         |
| [`==`](Lists.md#equality "Equality operator `==`")        | Evaluates to **true** if both *size*s are equal and all *element*s with corresponding *index*es are equal.            |
| [`!=`](Lists.md#inequality "Inequality operator `!=`")    | Evaluates to **true** whether both *size*s are not equal or if any *element* with corresponding *index* is not equal. |
| [`in`](Lists.md#in "Set membership operator `in`")        | Evaluates to **true** if *element* on the LHS of `in` is exists in the list.                                          |
| [`foreach`](Lists.md#foreach "`foreach` statement")       | Iterates over the list's *element*s.                                                                                  |

## List Methods
| Method    | Description   |
| :-------- | :------------ |
| [int `size()`](Lists.md#size "function int `size()`")                                                             | Returns the number of *element*s in the list.                                                                 |
| [`clear()`](Lists.md#clear "function void `clear()`")                                                             | Removes all *element*s from the list.                                                                         |
| [&lt;data_type&gt; `delete(int index)`](Lists.md#delete "function &lt;data_type&gt; `delete(intindex)`")          | Moves out the *element* at the specified *index*, which must be a non-negative integer.                       |
| [`insert(int index, data_type element)`](Lists.md#insert "function void `insert(int index, data_type element)`")  | Adds the *element* to the specified *index*, and all *element*s at and beyond the *index* are moved by one.   |
| [&lt;data_type&gt; `pop_front()`](Lists.md#pop_front "function &lt;data_type&gt; `pop_front()`")                  | Moves out the first *element* from the list. Same as `delete(0)`.                                             |
| [`push_front(data_type element)`](Lists.md#push_front "function void `push_front(data_type element)`")            | Adds the *element* to the beginning of the list. Same as `insert(0, element)`.                                |
| [&lt;data_type&gt; `pop_back()`](Lists.md#pop_back "function &lt;data_type&gt; `pop_back()`")                     | Moves out the last *element* from the list. Same as `delete(size()-1)`.                                       |
| [`push_back(data_type element)`](Lists.md#push_back "function void `push_back(data_type element)`")               | Adds the *element* to the end of the list. Same as `insert(size()-1, element)`.                               |
| [set&lt;data_type&gt; `to_set()`](Lists.md#to_set "function set&lt;data_type&gt; `to_set()`")                     | Returns all *element*s to a `set`-type.                                                                       |
| [`shuffle()`](Lists.md#shuffle "function void `shuffle()`")<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.3.0</span></span> <span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span>    | Randomizes orders of *element*s.                                                                              |

---

## Index operator `[]` {#index}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* for index operator `[]`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* for index operator `[]`.

Used to access a specific *element* of a list by given *index*, which must be a non-negative integer.
```sv linenums="1"
list<int> intList = {1, 2, 3};

int intVal = intList[1];    //  intVal: 0 -> 2
int intVal = intList[9];    //  ILLEGAL (1)
```

1. The *index* is out of bounds.

!!! Warning
    *Index* should smaller than `size()` of the list.

## Assignment operator `=` {#assignment}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* for assignment operator `=`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* for assignment operator `=`.

Create a copy of the `list`-type expression on the RHS and assigns it to the list on the LHS.
```sv linenums="1"
list<int> intList = {1, 2, 3};

intList = {2, 3, 4};        //  intList: {1, 2, 3} -> {2, 3, 4}
```

???+ Note
    Operator that modify contents can only be used within `exec` block or native `function`.

## Equality operator `==` {#equality}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluates to **true** if both *size*s are equal and all *element*s with corresponding *index*es are equal.
```sv linenums="1"
list<int   > intList_0  = { 1 ,  2 ,  3      };
list<int   > intList_1  = { 1 ,  2 ,  3 ,  4 };
list<string> stringList = {"1", "2", "3"};

bit bitVal_0, bitVal_1, bitVal_2;
if (intList_0 == intList_0 ) bitVal_0 = 1;  //  bitVal_0: 0 -> 1; (1)
if (intList_0 == intList_1 ) bitVal_1 = 1;  //  bitVal_1: 0 -> 0; (2)
if (intList_0 == stringList) bitVal_2 = 1;  //  ILLEGAL (3)
```

1. Equalize *size* and all *element*s with corresponding *index*es.
2. Inequalize *size*.
3. Different *data_type* are incomparable.

!!! Warning
    Different *data_type* of two lists should **NOT** be compared.

## Inequality operator `!=` {#inequality}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluates to **true** whether both *size*s are not equal or if any *element* with corresponding *index* is not equal.
```sv linenums="1"
list<int   > intList_0  = { 1 ,  2 ,  3      };
list<int   > intList_1  = { 1 ,  2 ,  3 ,  4 };
list<string> stringList = {"1", "2", "3"};

bit bitVal_0, bitVal_1, bitVal_2;
if (intList_0 != intList_0 ) bitVal_0 = 1;  //  bitVal_0: 0 -> 0 (1)
if (intList_0 != intList_1 ) bitVal_1 = 1;  //  bitVal_1: 0 -> 1 (2)
if (intList_0 != stringList) bitVal_2 = 1;  //  ILLEGAL (3)
```

1. Equalize *size* and all *element*s with corresponding *index*es.
2. Inequalize *size*.
3. Different *data_type* are incomparable.

!!! Warning
    Different *data_type* of two lists should **NOT** be compared.

## Set membership operator `in` {#in}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluates to **true** if *element* on the LHS of `in` is exists in the list.
```sv linenums="1"
list<int> intList = {1, 2, 3};

bit bitVal_0, bitVal_1, bitVal_2;
if ( 1  in intList) bitVal_0 = 1;           //  bitVal_0: 0 -> 1
if ( 0  in intList) bitVal_1 = 1;           //  bitVal_1: 0 -> 0
if ("1" in intList) bitVal_2 = 1;           //  ILLEGAL (1)
```

1. Different *data_type* between LHS of `in` and the list's *element* on RHS of `in`.

!!! Warning
    *Data_type* of *element* on LHS of `in` should be **SAME** as the list's *element* on RHS of `in`.

## `foreach` statement {#foreach}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Iterates over the list's *element*s.

Look at [Procedural/`foreach`](../Procedural/index.md#foreach) for more information.
```sv linenums="1"
list<int> intList = {4, 5, 6};
int intVal_0 = 0;
int intVal_1 = 0;
int intVal_2 = 0;

foreach (i : intList[j]) {
    intVal_0 = i;           //  intVal_0: 0 -> 4 -> 5 -> 6
    intVal_1 = j;           //  intVal_1: 0 -> 0 -> 1 -> 2
    intVal_2 = intList[j];  //  intVal_2: 0 -> 4 -> 5 -> 6
}
```

---

## function int `size()` {#size}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns the number of *element*s in the list.
```sv linenums="1"
list<int> intList = {1, 2, 3};

int intVal = intList.size();        //  intVal: 0 -> 3
```

## function void `clear()` {#clear}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Removes all *element*s from the list.
```sv linenums="1"
list<int> intList = {1, 2, 3};

intList.clear();                    //  intList: {1, 2, 3} -> {}
```

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function &lt;data_type&gt; `delete(index)` {#delete}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Moves out the *element* at the specified *index*, which must be a non-negative integer.
```sv linenums="1"
list<int> intList = {1, 2, 3};

int intVal = intList.delete(1);     //  intVal: 0 -> 2; intList: {1, 2, 3} -> {1, 3}
int intVal = intList.delete(5);     //  ILLEGAL (1)
```

1. The *index* is out of bounds.

!!! Warning
    The *index* must be smaller than `size()` of the list.

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function void `insert(index, element)` {#insert}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* of `insert()`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* of `insert()`.

Adds the *element* to the specified *index*, and all *element*s at and beyond the *index* are moved by one.
```sv linenums="1"
list<int> intList = {1, 2, 3};

intList.insert(1, 6);               //  intList: {1, 2, 3} -> {1, 6, 2, 3}
intList.insert(4, 7);               //  intList: {1, 6, 2, 3} -> {1, 6, 2, 3, 7}
intList.insert(9, 8);               //  ILLEGAL (1)
intList.insert(0, "1");             //  ILLEGAL (2)
```

1. The *index* is larger than `size()` of the list.
2. The *data_type* of *element*s are not the same.

!!! Warning
    The *index* should **NOT** larger than `size()` of the list.

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function &lt;data_type&gt; `pop_front()` {#pop_front}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Moves out the first *element* from the list. Same as `delete(0)`.
```sv linenums="1"
list<int> intList = {1, 2, 3};

int intVal = intList.pop_front();   //  intVal: 0 -> 1; intList: {1, 2, 3} -> {2, 3}
```

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function void `push_front(element)` {#push_front}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* of `push_front()`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* of `push_front()`.

Adds the *element* to the beginning of the list. Same as `insert(0, element)`.
```sv linenums="1"
list<int> intList = {1, 2, 3};

intList.push_front(4);              //  intList: {1, 2, 3} -> {4, 1, 2, 3}
```

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function &lt;data_type&gt; `pop_back()` {#pop_back}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Moves out the last *element* from the list. Same as `delete(size()-1)`.
```sv linenums="1"
list<int> intList = {1, 2, 3};

int intVal = intList.pop_back();    //  intVal: 0 -> 3; intList: {1, 2, 3} -> {1, 2}
```

???+ Note
    Method that modify contents can be used within `exec` block or native `function`.

## function void `push_back(element)` {#push_back}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *element* of `push_back()`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *element* of `push_back()`.

Adds the *element* to the end of the list. Same as `insert(size()-1, element)`.
```sv linenums="1"
list<int> intList = {1, 2, 3};

intList.push_back(4);               //  intList: {1, 2, 3} -> {1, 2, 3, 4}
```

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function set&lt;data_type&gt; `to_set()` {#to_set}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns all *element*s to a `set`-type.
```sv linenums="1"
list<int> intList = {1, 2, 1};

set<int> intSet = intList.to_set(); //  intSet: {} -> {1, 2}
```

## function void `shuffle()` {#shuffle}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.3.0</span></span>
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span>

Randomizes orders of *element*s.
```sv linenums="1"
list<int> intList = {1, 2};

intList.shuffle();                  //  intList: {1, 2} -> {1, 2} or {2, 1}
```

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

???+ Tip "Usage: use `shuffle()` to generate a unique random array"
    While `unique` become larger, the random solver can't solved within a limit iteration. Using `shuffle()` and `repeat()` can generate unique element for each variable.

    === ":fontawesome-regular-face-frown:{.red} Using `unique`"
        ```sv linenums="1"
        array<bit [4], 4> nibbleArray;
        constraint {
            unique {nibbleArray[0], nibbleArray[1], nibbleArray[2], nibbleArray[3]};
        }
        ...
        //  do something with solved nibbleArray
        ```

    === ":fontawesome-regular-face-smile:{.green} Using `shuffle`"
        ```sv linenums="1"
        array<bit [4], 4> nibbleArray;
        list <bit [4]   > element_pool;
        exec pre_solve {
            repeat (i : 16) element_pool.push_back(i);  //  element_pool: {} -> {0..15}
            element_pool.shuffle();
            foreach(nibbleArray[i]) nibbleArray[i] = element_pool.pop_front();
        }
        ...
        //  do something with solved nibbleArray
        ```
