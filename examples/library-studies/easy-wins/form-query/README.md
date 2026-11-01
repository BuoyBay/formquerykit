# Form and query encoding

This console example preserves duplicate tags, distinguishes bare and explicitly
empty fields, decodes UTF-8 and prints form and paired-query encodings. It
performs no network request or form submission.

From this directory:

```sh
bpm install
mkdir -p dist
bpm build main.bui -o dist/example
./dist/example
```

BPM records the selected package commit in its lockfile. Read the
[package API](../../../../docs/API.md) for component-only input, strict decoding
and size limits.
