# HTML forms and query pairs

`Import formquerykit` provides ordered UTF-8 name/value codecs for HTML form
bodies and paired URI query components. It has no native or package dependencies,
performs no network requests and does not mutate a URI or submit a form.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add formquerykit --git https://github.com/BuoyBay/formquerykit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer formquerykit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
