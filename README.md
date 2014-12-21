[![Build Status][travis-image]][travis-link]
# Rust by Example

## What's this?

This is the source code of the
[Rust by example][website] website!

## How to contribute

See [CONTRIBUTING.md][how-to-contribute].

## How to generate the static site

```
make all
make book
make test
```

View the results with `make serve`.

Note: on Ubuntu `node` may be called `nodejs`. I had to edit `.bin/gitbook` accordingly.

### Details

We use these tools to generate the static site:

* [Rust][rust-lang] \o/
* [gitbook][gitbook]

`gitbook` will generate the site from markdown files (see details about how it
works [here][gitbook-format]).

Before running `gitbook`, we do a preprocessing step using
[src/update.rs][update-rs].

This preprocessing has two steps:

### Generating the `SUMMARY.md`

`SUMMARY.md` is generated from the
[examples/structure.json][structure] file. This JSON file
contains a tree-like structure of "examples".

Each example has:

* an id, e.g. `hello`
* a title, e.g. `Hello World`
* optionally, children, which is a vector of sub-examples, e.g. `null`
* a directory under `examples`, e.g. [examples/hello][hello-folder]
* an entry in examples/structure.json, e.g.
  `{ "id": "hello", "title": "Hello World", "children": null }`
* some source file(s), e.g. [examples/hello/hello.rs][hello-rs]
* an input markdown file, e.g.
  [examples/hello/input.md][hello-md]

When dealing with a child example, the path will have to include the id of its
ancestors; e.g. `examples/variable/mut/input.md`, implies that a `mut` example
lives under the `variable` example.

### Processing `input.md`

Instead of including the rust code directly in `input.md`, the code lives in
separate source files; and the preprocessing step will insert the source code
in the markdown file.

For example, to insert the source code of the `hello.rs` file, the following
syntax is used in the markdown file:

* `{hello.play}` expands the source code embedded in a live code editor
* `{hello.rs}` expands to static/plain source code.
* `{hello.out}` expands to the output of executing the source code.

The Makefile provides the following recipes:

* `make`: builds `update.rs` and does the preprocessing step
* `make book`: runs `gitbook` to generate the book
* `make serve`: runs `gitbook --serve` to generate the book and publishes it
  under `localhost:4000`
* `make test`: will check all the rust source files for compilation errors

## License

Rust by example is dual licensed under the Apache 2.0 license and the MIT
license.

See LICENSE-APACHE and LICENSE-MIT for more details.

[travis-image]: https://travis-ci.org/yAngle/rust-by-example.svg?branch=master
[travis-link]: https://travis-ci.org/yAngle/rust-by-example
[website]: http://rustbyexample.com
[how-to-contribute]: CONTRIBUTING.md
[rust-lang]: http://www.rust-lang.org/
[gitbook]: http://www.gitbook.io
[gitbook-dir]: https://github.com/GitbookIO/gitbook#book-format
[update-rs]: src/update.rs
[structure]: examples/structure.json
[hello-folder]: examples/hello
[hello-rs]: examples/hello/hello.rs
[hello-md]: examples/hello/input.md

