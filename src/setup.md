# Environment Setup

To get a more developer-like experience, you'd need to install rust toolchain on your machine.

Install [Rustup](https://www.rust-lang.org/tools/install) toolchain installer, the site suggests the following command for linux

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

For windows check [rust in windows](https://learn.microsoft.com/windows/dev-environment/rust/setup) you'd need also need to install Vistual Studio build tools.

This rustup tool that can manage multiple versions of Rust compiler on a machine. To check installation and to update the compiler version run: `rustup update`

If you want to develop inside a dev container, you can create one by following through this [tutorial](https://code.visualstudio.com/docs/devcontainers/containers). In short install and configure Docker in your system, install "Dev Container" extension in VSCode and add and edit a dev container configuration file to the repo and rebuild the container to get started.

Alternatively you can try code in [Online Rust Playground](https://play.rust-lang.org/)

# IDEs and Tools

There are different IDE available for rust development. Visual Studio Code is the most popular one. Jetbrain also provide a separate IDE [RustRover](https://www.jetbrains.com/rust/)

To setup for rust install `rust-analyzer` for VScode. It will provide you with code completion, syntax highlighting, and other features. The toolset also includes linting provided by rustc and clippy to detect issues with your code.

If you are using vim you will have to install [rust-analyzer](https://github.com/rust-lang/rust-analyzer) from rustup and [rust.vim](https://github.com/rust-lang/rust.vim)


### [Cargo](https://doc.rust-lang.org/cargo/index.html) : Rust build tool, dependency manager and package manager

`rustc` is the Rust compiler that allows us to compile the file, but managing multiple files and dependencies can be difficult hence we use Cargo.

- **Start a project** :  `cargo new prj_name`
  
  creates a folder with `Cargo.toml` manifest files to track metadata and dependencies and `src/main.rs` with hello world script
  - initialize with a new git repo as well `cargo new --vcs=git prj`
  - creating a binary executable project : `cargo new prj_name --bin` 
  - creating a library project `cargo new prj_name --lib` --> creates `lib.rs` instead of `main.rs`

- Quickly check code to make sure it compiles without producing an executable `cargo check`

- [**Add Dependencies**](https://doc.rust-lang.org/cargo/commands/cargo-add.html):
  - `cargo add crate_name`: This will add the latest crate from the package registry [Crates.io](https://crates.io/) to the `Cargo.toml` file. To use the library you'll have to import the page in your source code: `use crate_name::func;` All crates on crates.io will automatically have its documentation built and available on docs.rs


  eg:  `cargo add rand` To use dependencies in the code add the following line

    ```rust
    use rand::Rng;

    let rand_num = rand::thread_rng().gen_range(1..=100);
    ```

  - use `--git URL --branch branch --tag tag` to add crate from git URL branch and tag
  - use `--dev` add as dev dependency, `--build` to add as build dependency, `--target` add as target dependency to the given target platform
  - use `-F features` or `--feature features` feature to add/activate additional features available in the crate

- **Build your project**: `cargo build`

  compiles into a target/debug folder with compiled file and other build files
  - binaries can also be generate using `rustc main.rs`, but it becomes easier to build with cargo with multiple files and dependencies
  - building for release compiles with optimization to target/release `cargo build --release`
  - build also creates cargo.lock file that figure out and stores the versions of dependencies that fits the criteria and ensures we rebuild the same artifact every time
- `cargo update` will ignore the Cargo.lock file and update a crate to newer version that fit the specification in Cargo.toml

- **Compile and Run the project**: `cargo run`

- **Test your project**: `cargo test` 

- **Build documentation for your project**: `cargo doc --open` also opens the doc in a browser

  builds into `target\doc` compiling doc comments `///` from code into documentation 
  
- **Remove generated artifacts**: `cargo clean`

  removes `/target` folder

- **Publish a library to crates.io**: `cargo publish` 

Cargo is extensible with sub command plugins such as:

- Linter: [Clippy](https://github.com/rust-lang/rust-clippy): install using `rustup component add clippy` and run (and automatically fix )using `cargo clippy --fix`
- Formatter: [Rustfmt](https://github.com/rust-lang/rustfmt): auto-formatting to community standards, run using `cargo fmt`

Other advance features: workspaces, build scripting.
