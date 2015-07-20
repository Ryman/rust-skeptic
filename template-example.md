# Skeptic Template Example

This is an example of [Rust Skeptic
Templates](README.md#skeptic-templates). See
[README.md](README.md) for the main documentation.

Templates allow you to explicitly perform some of the automatic
transformations that rustdoc does on code examples.

In this document we will automatically inject `extern crate skeptic;`,
turn off unused variable warnings, and wrap the example in main:

<code>```rust,skeptic-template</code>
```rust,skeptic-template
#![allow(unused_variables)]
extern crate skeptic;

fn main() {{
   {}
}}

```
<code>```</code>

Note that this is a [Rust format
specifier](http://doc.rust-lang.org/std/fmt/index.html), so braces are
treated specially, and need to be escaped with double-braces.

Now, examples we write here can take some shortcuts:

```rust,ignore
use skeptic::generate_doc_tests;

let unused_dont_care = 0;
```
