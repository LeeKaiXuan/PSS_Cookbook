---
title: Map
description: PSSv3.0/7.9.4 Maps
---

# Map {#map}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

## Properties
- Unordered.
- Non-randomizable.
- *Key*, *element* and *data_type* can be any [*scalar*](../DataTypes/index.md#datatypes_scalar "e.g., `bit`, `int`, `bool`, `enum`, `string`, `float32`, `float64`, `chandle`") or [*aggregate*](../DataTypes/index.md#datatypes_aggregate "e.g., `array`, `list`, `map`, `set`, `struct`") of scalars.
- Each *key* must be unique.
- Can be nested by any collection types (e.g., `array`, `list`, `map` or `set`).

## Declarations
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `chandle` as *data_type*.

Map can be declared by following syntax:<br>
> map&lt;*data_type*, *data_type*&gt; *identifier*

```sv linenums="1"
map<bit [4] , int    > nibble2int  ;    //  nibble2int  : {}
map<int     , bool   > int2bool    ;    //  int2bool    : {}
map<bool    , string > bool2string ;    //  bool2string : {}
map<string  , float32> string2float;    //  string2float: {}
map<float32 , int    > float2int   ;    //  float2int   : {}
map<eSTR2NUM, int    > enum2int    ;    //  enum2int    : {} (1)
```

1. Assume defined enum type before
```sv linenums="1"
enum eSTR2NUM {
    ZERO, ONE, TWO
};
```

## Initialization Assignment
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *key* and *element* in initialization assignment.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *key* or *element* in initialization assignment.

Map can be assigned at declaration; otherwise, it will be initialized to empty aggregate literal (`{}`).
```sv linenums="1"
map<bit [4] , int    > nibble2int   = {4'b1110:3      , 4'b0110:2     };
map<int     , bool   > int2bool     = {      4:true   ,       5:false };
map<bool    , string > bool2string  = {  false:"FALSE",    true:"TRUE"};
map<string  , float32> string2float = {  "2.1":2.1    ,   "2.2":2.2   };
map<float32 , int    > float2int    = {    2.4:2      ,     2.6:3     };
map<eSTR2NUM, int    > enum2int     = {    ONE:1      ,     TWO:2     }; // (1)!
```

1. Assume defined enum type before
```sv linenums="1"
enum eSTR2NUM {
    ZERO, ONE, TWO
};
```

## Map Operators
| Operator                                              | Description                                                                                                           |
| :---------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------- |
| [`[]`](Maps.md#index "Index operator `[]`")           | Used to access a specific *element* of a map by given *key*.                                                          |
| [`=`](Maps.md#assignment "Assignment operator `=`")   | Create a copy of the `map`-type expression on the RHS and assigns it to the map on the LHS.                           |
| [`==`](Maps.md#equality "Equality operator `==`")     | Evaluates to **true** if both *size*s are equal and all *element*s with corresponding *key*s are equal.               |
| [`!=`](Maps.md#inequality "Inequality operator `!=`") | Evaluates to **true** whether both *size*s are not equal or if any *element* with corresponding *key* is not equal.   |
| [`foreach`](Maps.md#foreach "`foreach` statement")    | Iterates over the map's *element*s.                                                                                   |


## Map Methods
| Method                                                                                            | Description                                                                   |
| :------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------- |
| [int `size()`](Maps.md#size "function int `size()`")                                              | Returns the number of *element*s in the map.                                  |
| [`clear()`](Maps.md#clear "function void `clear()`")                                              | Removes all *element"s from the map.                                          |
| [&lt;data_type&gt; `delete(key)`](Maps.md#delete "function &lt;data_type&gt; `delete(key)`")      | Moves out the *element* by the specified *key*, which must exists in the map. |
| [`insert(key, element)`](Maps.md#insert "function void `insert(key, element)`")                   | Adds the *element* with the specified *key*.                                  |
| [set&lt;data_type&gt; `keys()`](Maps.md#keys "function set&lt;data_type&gt; `keys()`")            | Returns all *key*s to a `set`-type.                                           |
| [list&lt;data_type&gt; `values()`](Maps.md#values "function list&lt;data_type&gt; `values()`")    | Returns all *element*s to a `list`-type.                                      |

---

## Index operator `[]` {#index}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *key* and *element* for index operator `[]`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *key* or *element* for index operator `[]`.

Used to access a specific *element* of a map by given *key*.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2};

int intVal = string2int["2"];   //  intVal: 0 -> 2;
int intVal = string2int["3"];   //  ILLEGAL (1)
int intVal = string2int[ 1 ];   //  ILLEGAL (2)
```

1. The *key* `"3"` not exist.
2. The *data_type* of *index* and *key* are not the same.

!!! Warning
    When used to reference an *element* in the map, the *key* should be exists.

???+ Note
    When used at LHS for assignment, the *key* will be added if not exists.
    ```sv linenums="1"
    map<string, int> string2int = {"1":1, "2":2};

    string2int["3"] = 3;    //  string2int: {"1":0, "2":2} -> {"1":0, "2":2, "3":3}
    ```
???+ Note
    Operator that modify contents can only be used within `exec` block or native `function`.

## Assignment operator `=` {#assignment}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *key* and *element* for assignment operator `=`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *key* or *element* for assignment operator `=`.

Creates a copy of the `map`-type expression on the RHS and assigns it to the map on the LHS.
Last *element* will be used if there have multiple *element*s with same *key* in the expression.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2};

string2int = {"3":3, "4":4};    //  string2int: {"1":1, "2":2} -> {"3":3, "4":4}
string2int = {3:"3", 4:"4"};    //  ILLEGAL (1)
```

1. The `data_type`s are not same as declaration of the map.

???+ Note
    Operator that modify contents can only be used within `exec` block or native `function`.

## Equality operator `==` {#equality}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluates to **true** if both *size*s are equal and all *element*s with corresponding *key*s are equal.
```sv linenums="1"
map<string, int   > map_0 = {"1":1  , "2":2  };
map<string, int   > map_1 = {"1":0  , "2":2  };
map<string, int   > map_2 = {"0":1  , "2":2  };
map<string, int   > map_3 = {"2":2  , "1":1  };
map<string, int   > map_4 = {"1":1           };
map<int   , string> map_5 = {  1:"1",   2:"2"};

bit bitVal_0, bitVal_1, bitVal_2, bitVal_3, bitVal_4, bitVal_5;
if (map_0 == map_0) bitVal_0 = 1;   //  bitVal_0: 0 -> 1 (1)
if (map_0 == map_1) bitVal_1 = 1;   //  bitVal_1: 0 -> 0 (2)
if (map_0 == map_2) bitVal_2 = 1;   //  bitVal_2: 0 -> 0 (3)
if (map_0 == map_3) bitVal_3 = 1;   //  bitVal_3: 0 -> 1 (4)
if (map_0 == map_4) bitVal_4 = 1;   //  bitVal_4: 0 -> 0 (5)
if (map_0 == map_5) bitVal_5 = 1;   //  ILLEGAL (6)
```

1. Equalize *size* and all *element*s with corresponding *key*s.
2. Inequalize any *element* with corresponding *key*.
3. Inequalize any *element* with corresponding *key*.
4. Equalize *size* and all *element*s with corresponding *key*s.
5. Inequalize *size*.
6. Different *data_type* of *key* or *element* are incomparable.

!!! Warning
    Different *data_type* of *key* or *element* of two maps should **NOT** be compared.

## Inequality operator `!=` {#inequality}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Evaluates to **true** whether both *size*s are not equal or if any *element* with corresponding *key* is not equal.
```sv linenums="1"
map<string, int   > map_0 = {"1":1  , "2":2  };
map<string, int   > map_1 = {"1":0  , "2":2  };
map<string, int   > map_2 = {"0":1  , "2":2  };
map<string, int   > map_3 = {"2":2  , "1":1  };
map<string, int   > map_4 = {"1":1           };
map<int   , string> map_5 = {  1:"1",   2:"2"};

bit bitVal_0, bitVal_1, bitVal_2, bitVal_3, bitVal_4, bitVal_5;
if (map_0 != map_0) bitVal_0 = 1;   //  bitVal_0: 0 -> 0 (1)
if (map_0 != map_1) bitVal_1 = 1;   //  bitVal_1: 0 -> 1 (2)
if (map_0 != map_2) bitVal_2 = 1;   //  bitVal_2: 0 -> 1 (3)
if (map_0 != map_3) bitVal_3 = 1;   //  bitVal_3: 0 -> 0 (4)
if (map_0 != map_4) bitVal_4 = 1;   //  bitVal_4: 0 -> 1 (5)
if (map_0 != map_5) bitVal_5 = 1;   //  ILLEGAL (6)
```

1. Equalize *size* and all *element*s with corresponding *key*s.
2. Inequalize any *element* with corresponding *key*.
3. Inequalize any *element* with corresponding *key*.
4. Equalize *size* and all *element*s with corresponding *key*s.
5. Inequalize *size*.
6. Different *data_type* of *key* or *element* are incomparable.

!!! Warning
    Different *data_type* of *key* or *element* of two maps should **NOT** be compared.

## `foreach` statement {#foreach}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Iterates over the map's *element*s.

Look at [Procedural/`foreach`](../Procedural/index.md#foreach) for more information.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2};
int    intVal_0 = 0;
string intVal_1 = "";
int    intVal_2 = 0;

foreach (i : string2int[j]) {
    intVal_0 = i;               //  intVal_0: 0  ->  1  ->  2
    intVal_1 = j;               //  intVal_1: "" -> "1" -> "2"
    intVal_2 = string2int[j];   //  intVal_2: 0  ->  1  ->  2
}
```

---

## function int `size()` {#size}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Returns the number of *element*s in the map.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2};

int intVal = string2int.size(); //  intVal: 0 -> 2
```

## function void `clear()` {#clear}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Removes all *element*s from the map.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2};

string2int.clear(); //  string2int: {"1":1, "2":2} -> {}
```

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function &lt;data_type&gt; `delete(data_type key)` {#delete}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Moves out the *element* by the specified *key*, which must exists in the map.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2};

map.delete("1");    //  string2int: {"1":1, "2":2} -> {"2":2}
map.delete("3");    //  ILLEGAL (1)
```

1. The *key* `"3"` is not exist.

!!! Warning
    The *key* should be exists in the map.

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function void `insert(data_type key, data_type element)` {#insert}
!!! Success "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>"
    PSSGen: Support `bit`, `int`, and `string` as *key* and *element* of `insert()`.

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `bool`, `enum`, `float32`, `float64`, `chandle`, and `struct` as *key* or *element* of `insert()`.

Adds the *element* with the specified *key*.
The *element* will be replaced if the *key* already exists.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2};

string2int.insert("1", 0  );    //  string2int: {"1":1, "2":2} -> {"1":0, "2":2}
string2int.insert("3", 3  );    //  string2int: {"1":0, "2":2} -> {"1":0, "2":2, "3":3}
string2int.insert("4", 4.0);    //  ILLEGAL (1)
string2int.insert(5  , 5  );    //  ILLEGAL (2)
```

1. The *data_type* of *element* is not same.
2. The *data_type* of *key* is not same.

!!! Waring
    The *data_type* of *key* or *element* should be same as the map's declaration.

???+ Note
    Method that modify contents can only be used within `exec` block or native `function`.

## function set&lt;data_type&gt; `keys()` {#keys}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `float32`, `float64`, and `chandle` as *key* for `keys()` method.

Returns all *key*s to a `set`-type.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2, "2.5":2};

set<string> stringSet = string2int.keys();  //  stringSet: {} -> {"1", "2", "2.5"}
```

## function list&lt;data_type&gt; `values()` {#values}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support `float32`, `float64`, and `chandle` as *element* for `values()` method.

Returns all *element*s to a `list`-type.
```sv linenums="1"
map<string, int> string2int = {"1":1, "2":2, "2.5":2};

list<int> intList = string2int.values();    //  intList: {} -> {1, 2, 2}
```
