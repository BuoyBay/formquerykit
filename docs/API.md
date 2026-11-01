# FormQueryKit 0.1.0

`Import formquerykit` provides ordered UTF-8 name/value codecs for HTML form
bodies and paired URI query components. It has no native or package dependencies,
performs no network requests and does not mutate a URI or submit a form.

## API

- `New FormPair(name, value, hadEquals = True)` is immutable: `Name`, `Value`,
  `HadEquals`. Parsed `flag` has an empty value and `HadEquals = False`;
  `flag=` has an empty value and `HadEquals = True`.
- `ParseForm(sourceText, maxBytes = 65536, maxPairs = 1000)` returns a fresh
  `Array Of FormPair`. `+` decodes to space.
- `ParseQueryPairs(sourceText, maxBytes = 65536, maxPairs = 1000)` uses the same
  ampersand/first-equals convention, with literal `+` preserved.
- `WriteForm(pairs, maxBytes = 65536, maxPairs = 1000)` uses the HTML form encode
  set: ASCII alphanumerics and `*-._` remain literal, space becomes `+`, and all
  other UTF-8 bytes become uppercase `%HH`. In particular `~` becomes `%7E`.
- `WriteQueryPairs(...)` preserves only ASCII alphanumerics and `-._~`, encodes
  space as `%20` and literal plus as `%2B`.
- `FormValues(pairs, name)` returns all exact, case-sensitive matches in their
  original order; absent names return an empty array. `FormHasName(...)` tests
  presence, including a present empty value.

The caller owns returned arrays; pairs themselves are immutable. Duplicates,
empty names and empty values are preserved. Empty `&` sequences are skipped,
and each nonempty sequence splits at only its first `=`. Empty input has no
pairs. Percent decoding happens once, after framing; escaped `&`/`=` cannot
change pair boundaries. Raw Unicode component text is encoded as UTF-8 before
processing. U+FEFF is preserved as data, not stripped as a document BOM.

Supply only the query component, without a leading `?`, `#fragment` or URL.
These characters are ordinary data to this paired-query API. Use URIKit's
`URIReference.HasQuery` and `Query` first when parsing a full URI: an absent
query and an explicitly empty query both contain zero pairs, so that distinction
belongs to the URI reference. URIKit's generic percent decoder preserves plus;
HTML form decoding deliberately does not. This package does not interpret
semicolon separators, bracket/nested keys, `_charset`, duplicate-key overrides,
application-specific booleans or non-UTF-8 encodings.

Both writers always emit `name=value`, even for pairs originally written as
bare names. `HadEquals` supports application decisions after parsing; it is not
used by canonical serialization. Zero pairs serialize to empty text.

## Strict decoding and bounds

Serialization follows WHATWG's UTF-8 form encoding algorithm. Parsing is an
**explicit strict variant**: malformed `%HH` throws `InvalidArgumentException`,
invalid UTF-8 throws `EncodingException`, and decoded NUL throws
`InvalidArgumentException`. Browsers' forgiving malformed-percent handling and
UTF-8 replacement are intentionally not reproduced. This is not a claim of full
URLSearchParams compatibility. Buoy's current String runtime cannot losslessly
represent NUL; use a byte-oriented protocol API for arbitrary octets.

`maxBytes` must be in `1..1048576`; `maxPairs` in `1..10000`. Parse bounds apply
to the source's UTF-8 byte length before copying and to the number of emitted
pairs. Decoding cannot increase bytes. Write bounds apply to the total ASCII
output; individual fields are also checked before UTF-8 copying and encoded
fragments are bounded. A single field can still use substantially more memory
than its byte count because fragment arrays have overhead. Lookup accepts at
most 10000 pairs. Nil pairs reject. No partial result is returned after an error.
These are whole-component operations, not streaming codecs.

## Build and verification

```sh
/usr/bin/buoy pkg seal . -o packages/formquerykit-0.1.0.buipkg
```

The runner seals a fresh package, checks module/test/example across six desktop
targets, executes both consumers and checks generated valid inputs against
Python `urllib.parse`. The form oracle applies WHATWG's `*`/`~` encode-set
adjustment explicitly. Malformed-input strictness is tested separately.
`BUOY` selects the compiler and `BPM_PACKAGER` the sealing command. Native
execution is on Linux; other platforms are compiler checks. No remote package
publication was performed.

See the example (not included in this repository) and
[provenance](../PROVENANCE.md).

Test suites and fixtures are maintainer-local and are not part of this public repository.
