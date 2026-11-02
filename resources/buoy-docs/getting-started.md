---
title: "formquerykit Getting started"
summary: "Install, import, and explore the public formquerykit API."
category: "Guide"
symbols: ["FormPair"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Install the formquerykit package into your project’s packages directory. Import the module you need: `Import formquerykit`.

Start with the [API contracts](api.md) for usage, constraints, and platform limitations. The [typed API](typed-api.md) lists constructors, each parameter and its default, properties, and return values directly from source.

## Encode a value and handle rejected input

Pass decoded names and values to the writers; do not percent-encode them first.
This complete program shows how the same value is encoded for a form body and
for a query component:

```basic
Import formquerykit

Sub Main()
    Dim pairs As Array Of FormPair = [New FormPair("q", "C++ café")]
    Print WriteForm(pairs)
    Print WriteQueryPairs(pairs)

    Dim form As Array Of FormPair = ParseForm("q=C++")
    Dim query As Array Of FormPair = ParseQueryPairs("q=C++")
    Print "form: [" + form(0).Value + "]"
    Print "query: [" + query(0).Value + "]"

    Try
        Dim parsed As Array Of FormPair = ParseForm("q=%ZZ", maxBytes: 4096, maxPairs: 50)
        Print "Accepted pairs: " + String.From(Array.Count(parsed))
    Catch e As EncodingException
        Print "Invalid UTF-8: " + e.Message
    Catch e As InvalidArgumentException
        Print "Invalid form input or limits: " + e.Message
    End Try
End Sub
```

The first four lines are:

```text
q=C%2B%2B+caf%C3%A9
q=C%2B%2B%20caf%C3%A9
form: [C  ]
query: [C++]
```

The two spaces after `C` in the form result come from the two literal plus
characters. To receive literal plus characters in form input, send `%2B`.
Pass only the query component to `ParseQueryPairs`, without `?` or a full URL.

The final line reports the malformed `%ZZ` escape. `EncodingException` covers
invalid UTF-8; `InvalidArgumentException` also covers malformed escapes, decoded
NUL, invalid configured limits, and exceeded byte or pair limits. Catch these
at the input boundary and use `Message` for diagnostic context; the API does
not provide a structured error offset. No partial array is returned. The
example's 4096-byte limit applies to the UTF-8 source before decoding.

[Package overview](index.md)
