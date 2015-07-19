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

In `build.rs` write this to test all code blocks in `README.md`:

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

Now any Rust code blocks in `README.md` will be tested during `cargo test`.

# Users' Guide

Rust Skeptic is not based on rustdoc. It behaves similarly in many cases, but not all. Here's the lowdown on the Skeptic system.

*You must ask for `rust` code blocks explicitly to get Rust testing*, with <code>```rust</code>. This is different from rustdoc, which assumes code blocks are Rust. The reason for this is that common Markdown parsers, like that used on GitHub, also do not assume Rust by default: you either get both Rust syntax highlighting and testing, or no Rust syntax highlighting and testing.

*Note: [this `README.md` file itself is tested by Rust Skeptic](https://github.com/brson/rust-skeptic/blob/master/build.rs).* Because it is illustrating how to use markdown syntax, the markup *on this document itself* is funky, and so is the output below, particularly when illustrating Markdown's code fences (<code>```rust</code>). Use your imagination to pretend the 

<code>```rust,ignore</code>
```
fn do_amazing_thing() -> i32 {
   // TODO: How do I do this?
   unimplemented!()
}

do_amazing_thing();
```
<code>```</code>

# License

MIT/Apache-2.0
