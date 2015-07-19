# Be a Rust Skeptic

Test your Rust Markdown documentation via Cargo.

# Getting started

Put this in `Cargo.toml` to add the `skeptic` dependency:

```toml
[build-dependencies]
skeptic = "*"
```

Also in `Cargo.toml`, to the `[package]` section add:

```toml
build = "build.rs"
```

That adds a [build script](http://doc.crates.io/build-script.html)
through which you will tell Skeptic to build test cases from a set
of Markdown files.

In `build.rs` write this to test all Rust code blocks in `README.md`:

```rust
extern crate skeptic;

fn main() {
    skeptic::generate_doc_tests(&["README.md"]);
}
```

Finally, in `tests/skeptic.rs` put the following macros to tie the
generated test cases to `cargo test`:

```rust,ignore
include!(concat!(env!("OUT_DIR"), "/skeptic-tests.rs"));
```

Now any Rust code blocks in `README.md` will be tested during `cargo
test`.

# Users' Guide

Rust Skeptic is not based on rustdoc. It behaves similarly in many
cases, but not all. Here's the lowdown on the Skeptic system.

*Note: [this `README.md` file itself is tested by Rust
Skeptic](https://github.com/brson/rust-skeptic/blob/master/build.rs).
Because it is illustrating how to use markdown syntax, the markup on
this document itself is funky, and so is the output below,
particularly when illustrating Markdown's code fences
(<code>```rust</code>).*

*You must ask for `rust` code blocks explicitly to get Rust testing*,
with <code>```rust</code>. This is different from rustdoc, which
assumes code blocks are Rust. The reason for this is that common
Markdown parsers, like that used on GitHub, also do not assume Rust by
default: you either get both Rust syntax highlighting and testing, or
no Rust syntax highlighting and testing.

So the below is not tested by Skeptic.

<code>```</code>
```
let this_is_not_going_to_be_compiled_and_run = @all;
It doesn't really matter what's in here.
```
<code>```</code>

You must summon The Rust with the The Words of Exampling: <code>```rust</code>. Backtick, backtick, backtick, `rust`, like thus:

<code>```rust</code>
```rust
fn main() {
   println!("Calm your skepticism. This example is verified.");
}
```
<code>```</code>

Skeptic will interpret other words in the code block's 'info string',
which should be separated by comma, ',', to be
Github-compatible. These words change how the test is interpreted:
`ignore`, and `should_panic`.

`ignore` causes the test not to be run during testing, so it will neither pass nor fail.

<code>```rust,ignore</code>
```rust,ignore
fn do_amazing_thing() -> i32 {
   // TODO: How do I do this?
   unimplemented!()
}

fn main() {
   do_amazing_thing();
}
```
<code>```</code>

Furthermore, like rustdoc, `ignore`d test cases will not even be
compiled, so may contain compilation errors:

<code>```rust,ignore</code>
```rust,ignore
fn do_amazing_thing() -> i32 {
   // TODO: How do I do this?
   unimplemented! whatever I'm distracted, oh cookies!
```
<code>```</code>

*Note: GitHub doesn't understand comma-separated words, like
 `rust,ignore`, and will not syntax the above as Rust. If anybody
 knows how to work around this let me know.*

`should_panic` causes the test to only pass if it terminates because
of a `panic!()`.

<code>```rust,should_panic</code>
```rust,should_panic
fn main() {
   assert!(1 == 100);
}
```
<code>```</code>

## Skeptic Templates

Unlike rustdoc, *Skeptic does not modify examples before testing by
default*. Skeptic examples are placed in a '.rs' file, compiled, then
run.

This means that - *by default* - Skeptic examples require a `main`
function, as in all the examples above. Implicit wrapping of examples
in `main`, and custom injection of `extern crate` statements and crate
attributes are controlled through document-level templates.

Since the examples in this README run as written, it doesn't need a
template, but we can specifiy a no-op template like so:

<code>```rust,skeptic-template</code>
```rust,skeptic-template
{}
```
<code>```</code>

Templates are [Rust format
specifiers](http://doc.rust-lang.org/std/fmt/index.html) that must
take a single argument (i.e. they need to contain the string "{}". See
[the template example](template-example.html) for more on templates.

Rust Skeptic uses
[`pulldown-cmark`](https://github.com/google/pulldown-cmark) for
Markdown parsing.

# License

MIT/Apache-2.0
