# addr2line

[![](http://meritbadge.herokuapp.com/addr2line) ![](https://img.shields.io/crates/d/addr2line.svg)](https://crates.io/crates/addr2line)
[![](https://docs.rs/addr2line/badge.svg)](https://docs.rs/addr2line/)
[![Build Status](https://travis-ci.org/gimli-rs/addr2line.svg?branch=master)](https://travis-ci.org/gimli-rs/addr2line)
[![Coverage Status](https://coveralls.io/repos/github/gimli-rs/addr2line/badge.svg?branch=master)](https://coveralls.io/github/gimli-rs/addr2line?branch=master)

A cross-platform library for retrieving per-address debug information
from executables with DWARF debug symbols.

`addr2line` uses [`gimli`](https://github.com/gimli-rs/gimli) to parse
the debug symbols of an executable, and exposes an interface for finding
the source file, line number, and wrapping function for instruction
addresses within the target program. These lookups can either be
performed programmatically through `Context::find_location` and
`Context::find_frames`, or via the included example binary,
`addr2line` (named and modelled after the equivalent utility from
[GNU binutils](https://sourceware.org/binutils/docs/binutils/addr2line.html)).

# Quickstart

 - Add the [`object` crate](https://crates.io/crates/object) to your `Cargo.toml`
 - Add the [`addr2line` crate](https://crates.io/crates/addr2line) to your `Cargo.toml`
 - Add `extern crate object` and `extern crate addr2line` to your main crate entry file
 - Load the file and parse it with [`object::File::parse`](https://docs.rs/object/*/object/struct.File.html#method.parse)
 - Pass the parsed file to [`addr2line::Context::new` ](https://docs.rs/addr2line/*/addr2line/struct.Context.html#method.new)
 - Use [`addr2line::Context::find_location`](https://docs.rs/addr2line/*/addr2line/struct.Context.html#method.find_framefind_location)
   or [`addr2line::Context::find_frames`](https://docs.rs/addr2line/*/addr2line/struct.Context.html#method.find_frames)
   to look up debug information for an address

# Performance

The library aims to perform similarly to equivalent existing tools such
as `addr2line` from binutils, `eu-addr2line` from elfutils, and
`llvm-symbolize` from the llvm project.

Currently the library optimizes for memory over for speed when parsing
line number sequences.  In particular, the algorithm used can be slow
for large line sequences, but uses much less memory.  Note that LLVM
generates one line sequence per function, but gcc can include multiple
functions in each line sequence.

We haven't done extensive
benchmarking (yet), but the runtime and memory use results we observe
for one relatively large Rust application are quite promising:

![addr2line runtime](time.png)
![addr2line memory](memory.png)

## License

Licensed under either of

  * Apache License, Version 2.0 ([`LICENSE-APACHE`](./LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
  * MIT license ([`LICENSE-MIT`](./LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
