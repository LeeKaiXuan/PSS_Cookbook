---
title: Data Types
description: PSSv3.0/7. Data types
---

# Data Types

## *Scalar* Type {#datatypes_scalar}
| Scalar Type   | Randomizable  |
| :------------ | :-----------: |
| [`bit`](IntegerTypes.md#datatypes_integertypes_bit "bit")<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>          | :white_check_mark: Randomizable   |
| [`int`](IntegerTypes.md#datatypes_integertypes_integer "integer")<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>  | :white_check_mark: Randomizable   |
| `bool`<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>                                                     | :white_check_mark: Randomizable   |
| `enum`<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>                                                             | :white_check_mark: Randomizable   |
| `string`<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>                                                           | :white_check_mark: Randomizable   |
| `float32`<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span> <span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span>  |                                   |
| `float64`<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span> <span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span>  |                                   |
| `chandle`<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span>                                                  |                                   |

## *Aggregate* {#datatypes_aggregate}
| Aggregate Type    | Randomizable  |
| :---------------- | :------------ |
| [`array`](../Collections/Arrays.md#array)<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>  | :white_check_mark: Only for `bit`, `int`, `bool`, `enum`, `string`    |
| [`list`](../Collections/Lists.md#list)<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>     | :white_check_mark: Only for `bit`, `int`, `bool`, `enum`, `string`<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-remove-outline:{.red}](../index.md#symbol 'PSSGen: Not support yet')</span><span class="mdx-badge__text">Not support yet</span></span> <span class="mdx-badge"><span class="mdx-badge__icon">[:material-book-check-outline:{.green}](../index.md#symbol 'LRM: Minimum version')</span><span class="mdx-badge__text">v2.1</span></span> |
| [`map`](../Collections/Maps.md#map)<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>        |                                                                       |
| [`set`](../Collections/Sets.md#set)<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v2.2.0</span></span>        |                                                                       |
| `struct`<br><span class="mdx-badge"><span class="mdx-badge__icon">[:material-tag-check-outline:{.green}](../index.md#symbol 'PSSGen: Minimum version')</span><span class="mdx-badge__text">v1.0.0</span></span>                                   | :white_check_mark: Only for `bit`, `int`, `bool`, `enum`, `string`    |

!!! Note "*Aggregate* may be nested."

## *Non-aggregate* {#datatypes_non_aggregate}
- `component`
- `action`
- `buffer`
- `stream`
- `state`
- `resource`
