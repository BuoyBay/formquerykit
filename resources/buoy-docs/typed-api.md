---
title: "formquerykit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["FormPair"]
dependencies: []
symbols: ["FormPair"]
related: ["api.md", "getting-started.md", "index.md"]
---

# formquerykit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## FormPair

`Public Class FormPair`

One immutable ordered name/value pair. HadEquals records bare-name syntax.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Name` | `String` | Yes | No | — | — |
| `Value` | `String` | Yes | No | — | — |
| `HadEquals` | `Boolean` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(name As String, value As String, hadEquals As Boolean = True)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `name` | `String` | `ByVal` | Required |
| `value` | `String` | `ByVal` | Required |
| `hadEquals` | `Boolean` | `ByVal` | `True` |

Returns: no value.

## FormQueryKit functions

### ParseForm

`Public Function ParseForm(sourceText As String, maxBytes As Integer = 65536, maxPairs As Integer = 1000) As Array Of FormPair`

Strict UTF8 form parser. Plus means space; malformed escapes/UTF8 reject.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `sourceText` | `String` | `ByVal` | Required |
| `maxBytes` | `Integer` | `ByVal` | `65536` |
| `maxPairs` | `Integer` | `ByVal` | `1000` |

#### Returns

| Type |
| --- |
| `Array Of FormPair` |

### ParseQueryPairs

`Public Function ParseQueryPairs(sourceText As String, maxBytes As Integer = 65536, maxPairs As Integer = 1000) As Array Of FormPair`

Ampersand-separated query convention. Plus remains a literal plus.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `sourceText` | `String` | `ByVal` | Required |
| `maxBytes` | `Integer` | `ByVal` | `65536` |
| `maxPairs` | `Integer` | `ByVal` | `1000` |

#### Returns

| Type |
| --- |
| `Array Of FormPair` |

### WriteForm

`Public Function WriteForm(pairs As Array Of FormPair, maxBytes As Integer = 65536, maxPairs As Integer = 1000) As String`

Canonical form serialization always writes name=value, including empty values.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `pairs` | `Array Of FormPair` | `ByVal` | Required |
| `maxBytes` | `Integer` | `ByVal` | `65536` |
| `maxPairs` | `Integer` | `ByVal` | `1000` |

#### Returns

| Type |
| --- |
| `String` |

### WriteQueryPairs

`Public Function WriteQueryPairs(pairs As Array Of FormPair, maxBytes As Integer = 65536, maxPairs As Integer = 1000) As String`

Encode paired query components with %20 for spaces and %2B for literal plus.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `pairs` | `Array Of FormPair` | `ByVal` | Required |
| `maxBytes` | `Integer` | `ByVal` | `65536` |
| `maxPairs` | `Integer` | `ByVal` | `1000` |

#### Returns

| Type |
| --- |
| `String` |

### FormValues

`Public Function FormValues(pairs As Array Of FormPair, name As String) As Array Of String`

Exact case-sensitive lookup, preserving duplicate-value order.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `pairs` | `Array Of FormPair` | `ByVal` | Required |
| `name` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Array Of String` |

### FormHasName

`Public Function FormHasName(pairs As Array Of FormPair, name As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `pairs` | `Array Of FormPair` | `ByVal` | Required |
| `name` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |
