[![Build Status](https://travis-ci.com/mipli/sgf-parser.svg?branch=master)](https://travis-ci.com/mipli/sgf-parser)
[![Crate](https://img.shields.io/crates/v/sgf-parser.svg)](https://crates.io/crates/sgf-parser)
[![Docs](https://img.shields.io/badge/docs-latest-blue.svg?style=flat-square)](https://docs.rs/sgf-parser/latest/sgf_parser/)

# SGF Parser

A [SGF](https://www.red-bean.com/sgf/index.html) Parser for Rust. Supports all SGF properties, and tree branching.

**NOTE** when converting a `GameTree` to a string we convert all charset tokens to be UTF-8, since
that is the encoding for all strings in Rust.

Using `pest` for the actual parsing part.

# Development

Code quality is ensured by running both `cargo clippy` and `cargo fmt` on each commit. 

All code should also be unit tested.

# Example usage
```rust
use sgf_parser::*;

let sgf_source = "(;C[comment]EV[event]PB[black]PW[white];B[aa])";
let tree: Result<GameTree, SgfError> = parse(sgf_source);

let tree = tree.unwrap();
let unknown_nodes = tree.get_unknown_nodes();
assert_eq!(unknown_nodes.len(), 0);

let invalid_nodes = tree.get_invalid_nodes();
assert_eq!(invalid_nodes.len(), 0);

tree.iter().for_each(|node| {
  assert!(!node.tokens.is_empty());
});

let sgf_string: String = tree.into();
assert_eq!(sgf_source, sgf_string());
```
