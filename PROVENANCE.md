# Provenance

Independent Buoy implementation, no copied upstream source or native libraries.
API design builds on the earlier URIKit gap: generic percent decoding does not
supply an ordered name/value form codec.

Primary reference inspected:
[WHATWG URL Standard, application/x-www-form-urlencoded](https://url.spec.whatwg.org/#application/x-www-form-urlencoded),
including parsing, serialization and the form percent-encode set. Accessed during
this implementation pass. Repeated pairs, first-equals framing, skipped empty
sequences and the UTF-8 serialization set follow that specification. Strict
malformed escape/UTF-8/NUL rejection is a documented deviation; the literal-plus
paired-query mode is a deliberate convention rather than a generic URI rule.

The separate Python standard-library oracle tests valid-input interoperability;
it is not used as the implementation or as an oracle for deliberately stricter
malformed-input behavior. Form codec functions use Bytes rather than URIKit's
printable-ASCII byte decoder so literal Unicode form text can be accepted without
a package dependency.
