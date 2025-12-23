---
title: Procedural
description: PSSv3.0/22.7 Procedural constructs
---

# Procedural

## `void` Function Calls {#void}
Declare a function that no return value.

*WIP*

## `return` Statement {#return}
Returns a value to the caller from a function or `exec` block.
Syntax:
> return *expression* ;

- *expression* : An **optional** value need for returned to the caller.

## `repeat` Statement {#repeat}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>

Executes *procedural_stmt* repeatedly by specified the number of *iteration*.
Syntax:
> repeat (*index_identifier* : *expression*) *procedural_stmt*

- *index_identifier* : An **optional** index-variable identifier can be specified that ranges from `0` to `*iteration* - 1`.
- *expression* : Specify *iteration* by a non-negative integer (e.g., `int` or `bit`).
- *procedural_stmt* : Iterated by the number of *iteration*. Not executed if *iteration* is `0`.

```sv linenums="1"
int intVal = 0;

repeat (i : 3) {
    intVal = i;     //  intVal: 0 -> 1 -> 2 -> 3
}
```

## `repeat`-`while` Statement {#repeat_while}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Executes *procedural_stmt* repeatedly as long as the *expression* is **true**.
Syntax:
> repeat *procedural_stmt* while (*expression*);

- *procedural_stmt* : Iterated so long as the *expression* is **true**.
- *expression* : Evaluates **true** or **false** everytime after *procedural_stmt*.

```sv linenums="1"
int intVal = 1;

repeat {
    intVal += 1;    //  intVal: 1 -> 2
} while (intVal < 1);
```

## `while` Statement {#while}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Executes *procedural_stmt* repeatedly as long as the *expression* is **true**.
Syntax:
> while (*expression*) *procedural_stmt*

- *procedural_stmt* : Iterated so long as the *expression* is **true**.
- *expression* : Evaluates **true** or **false** everytime before *procedural_stmt*.

```sv linenums="1"
int intVal = 0;

while (intVal < 3) {
    intVal += 1;    //  intVal: 0 -> 1 -> 2 -> 3
}
```

## `foreach` Statement {#foreach}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

Executes *procedural_stmt* for each *element* of the *expression*.
Syntax:
> foreach (*iterator_identifier* : *expression* [*index_identifier*]) *procedural_stmt*

- *iterator_identifier* : An **optional** iterator-variable identifier with collection *element* type, an is an lias to current *element*.
- *index_identifier* : An **optional** index-variable identifier can be specified that ranges from `0` to `size() - 1` when *expression* is `array`-type or `list`-type; or can be specified of all *key*s when *expression* is `map`-type.
- *expression* : A collection type (e.g., `array`, `list`, `map`, or `set`).
- *procedural_stmt* : Iterated by the number of `size()` of the *expression*.

!!! Warning
    - For `set`-type, *index_identifier* should **NOT** be specified.
    - At least one of *iterator_identifier* or *index_identifier* should be specified.
    - Both *iterator_identifier* and *index_identifier* should **NOT** be changed inside *procedural_stmt*.

???+ Note
    - A colon (`:`) must be append after *iterator_identifier* if its specified.
    - A pair of square brackets (`[]`) must be used to contain *index_identifier* if its specified.
    - The order of *key*s are undetermined when *expression* is `map`-type.

```sv linenums="1"
array<bit [4], 3> nibbleArray = {4'd4, 4'd5, 4'd6};
bit [4] nibbleVal = 0;
int intVal = 0;

foreach (i : intArray[j]) {
    nibbleVal = i;  //  nibbleVal: 0 -> 4'd4 -> 4'd5 -> 4'd6
    intVal    = j;  //  intVal   : 0 ->   0  ->   1  ->   2
}
```

!!! Tip "Usage: iterating over a sequence without declare collection"
    When needing to iterate over a sequence of *element*s, the sequence can defined inside `foreach` directly.
    Even defined a collection inside `foreach` also acceptable.

    === ":fontawesome-regular-face-frown:{.red} Defined outside"
        ```sv linenums="1"
        array<int, 4> sequenceArray = {2, 4, 6, 8};
        foreach (i : sequenceArray[j]) {
            (void) printf(i);   //  i: 2 -> 4 -> 6 -> 8
        }
        ```

    === ":fontawesome-regular-face-smile:{.green} Defined inside"
        ```sv linenums="1"

        foreach (i : {2, 4, 6, 8}[j]) {
            (void) printf(i);   //  i: 2 -> 4 -> 6 -> 8
            (void) printf(j);   //  j: 0 -> 1 -> 2 -> 3
        }
        ```

    === ":cloud_tornado: Defined a collection inside"
        ```sv linenums="1"
        foreach (i : {"A":10, "B":11, "C":12, "D":13, "E":14, "F":15}[j]) {
            (void) printf(i);   //  i: 10  -> 11  -> 12  -> 13  -> 14  -> 15
            (void) printf(j);   //  j: "A" -> "B" -> "C" -> "D" -> "E" -> "F"
        }
        ```

## `if`-`else` Statement {#if_else}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>

Executes *procedural_stmt_for_true* when *expression* is **true**.
Syntax:
> if (*expression*) *procedural_stmt_for_true* else *procedural_stmt_for_false*

- *expression* : Evaluates **true** or **false**.
- *procedural_stmt_for_true* : Executed when the *expression* is **true**.
- *procedural_stmt_for_false* : An **optional** procedural will be executed when the *expression* is **false**.

```sv linenums="1"
int intVal = 3;
bit bitVal = 0;

if (intVal > 0) {
    bitVal = 1;     //  bitVal: 0 -> 1
}
else {
    bitVal = 0;
}
```

## `match` Statement {#match}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support used in `constraint` block.

Executes *procedural_stmt* when corresponding *range* is match to the *expression*.
Syntax:
> match (*expression*) {<br>
>   [*range*] : *procedural_stmt*<br>
>   [*range*] : *procedural_stmt*<br>
>   ...<br>
>   default : *procedural_stmt*<br>
> }

- *expression* : The item to be evaluated.
- *range* : An open range list is compared to *expression*.
- *procedural_stmt* : Executed when *expression* matches any one in corresponding *range*.
- default : An **optional** procedural will be executed when not any *range* was matched.

!!! Warning
    - Only one of *range*s should be matched.
    - The `default` must be exist and executed if no any *range* was matched.

```sv linenums="1"
int intVal = 3;
bit [4] nibbleVal = 0;

match (intVal) {
    [-100..-1]   : nibbleVal = 4'b0001;
    [0..9]       : nibbleVal = 4'b0010;
    [10, 12, 14] : nibbleVal = 4'b0100; //  nibbleVal: 0 -> 4'b0100
    [100..199]   : nibbleVal = 4'b1000;
    default      : nibbleVal = 4'b1111;
}
```

## `break` Statement {#break}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support used in `constraint` and `activity` block.

Continues execution after enclosing innermost loop construct.
Syntax:
> break;

!!! Warning
    The `break` statement may only appear within loop statements (`repeat`, `repeat`-`while`, `while`, or `foreach`).

```sv linenums="1"
int intVal = 1;

while (intVal > 0) {
    intVal += 1;    //  intVal: 0 -> 1 -> 2 -> 3
    if (intVal > 2) break;
}
```

## `continue` Statement {#continue}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>

!!! Failure "<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>"
    PSSGen: Not support used in `constraint` and `activity` block.

Continues next loop iteration.
Syntax:
> continue;

!!! Warning
    The `continue` statement may only appear within loop statements (`repeat`, `repeat`-`while`, `while`, or `foreach`).

```sv linenums="1"
int intVal = 0;

repeat (i : 3) {
    if (i < 2) continue;
    intVal = i;     //  intVal: 0 -> 2
}
```

## `randomize` Statement {#randomize}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span>

Randomize the specified data attributes or variables.

Syntax:
> randomize *variable*, .., *variable* with *constraints*

- *variable*: A set variables of plain-data type to randomized together.
- *constraints*: **Optional** constraint rules.

```sv linenums="1"
bit [4] nibbleVal_0 = 0;
bit [4] nibbleVal_1 = 0;

exec post_solve {
    randomize nibbleVal_0, nibbleVal_1 with {nibbleVal_0 < nibbleVal_1;}
}
```

## `exec` Block {#exec}
<span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>

```sv linenums="1"
int intVal = 0;

exec body {
    intVal = 2; //  intVal: 0 -> 2
}
```
