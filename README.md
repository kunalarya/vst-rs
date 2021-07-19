# rust-vst
[![Travis Build][trav-img]][trav-url]
[![Appveyor Build][appv-img]][appv-url]
[![crates.io][crates-img]][crates-url]

A library to help facilitate creating VST plugins in rust.

This library is a work in progress and as such does not yet implement all
opcodes. It is enough to create basic VST plugins without an editor interface.

Note that this is a fork of the original `vst-rs` crate pre-1.0, and thus
users are responsible for parameter safety (namely accessing/mutating parameters
from different threads).

This library is been kept up-to-date with upstream on a best effort basis.

## Library Documentation
  * https://rust-dsp.github.io/rust-vst

## Crate

To use this crate, get it directly from Github.
```toml
# get directly from Github.  This will be unstable!
vst = { git = "https://github.com/kunalarya/rust-unsafe-vst" }
```

## Usage
To create a plugin, simply create a type which implements `plugin::Plugin` and
`std::default::Default`. Then call the macro `plugin_main!`, which will export
the necessary functions and handle dealing with the rest of the API.

## Example Plugin
A simple plugin that bears no functionality. The provided Cargo.toml has a
crate-type directive which builds a dynamic library, usable by any VST host.

`src/lib.rs`

```rust
#[macro_use]
extern crate vst;

use vst::plugin::{Info, Plugin};

#[derive(Default)]
struct BasicPlugin;

impl Plugin for BasicPlugin {
    fn get_info(&self) -> Info {
        Info {
            name: "Basic Plugin".to_string(),
            unique_id: 1357, // Used by hosts to differentiate between plugins.

            ..Default::default()
        }
    }
}

plugin_main!(BasicPlugin); // Important!
```

`Cargo.toml`

```toml
[package]
name = "basic_vst"
version = "0.0.1"
authors = ["Author <author@example.com>"]

[dependencies]
vst = { git = "https://github.com/rust-dsp/rust-vst" }

[lib]
name = "basicvst"
crate-type = ["cdylib"]
```

#### Packaging on OS X

On OS X VST plugins are packaged inside of loadable bundles. 
To package your VST as a loadable bundle you may use the `osx_vst_bundler.sh` script this library provides. 

Example: 

```
./osx_vst_bundler.sh Plugin target/release/plugin.dylib
Creates a Plugin.vst bundle
```

## Special Thanks
The authors of the `vst-rs` crate.
[Marko Mijalkovic](https://github.com/overdrivenpotato) for [initiating this project](https://github.com/overdrivenpotato/rust-vst2)
