# Packages, Crates and Modules

As program grows its importante to group related functionality and group separate code with distict features. Code organization in Rust is done using the concept of packages, crates and modules. 

Project code iss split into multiple modules and each module is a separate file. A module is a file that contains one or more items, and the items can be functions, structs, enums, constants, etc.

A package is a collection of crates. A crate is a binary or library that is built using the Rust compiler.

Packages: A Cargo feature that lets you build, test, and share crates
Crates: A tree of modules that produces a library or executable
Modules and use: Let you control the organization, scope, and privacy of paths
Paths: A way of naming an item, such as a struct, function, or module