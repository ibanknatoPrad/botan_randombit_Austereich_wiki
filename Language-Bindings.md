Though Botan is written in C++ it is possible to use it from some other languages including

### Ready To Use

* C89 - Available out of the box in the header `ffi.h`. This C interface is also intended to be the preferred way of binding Botan to other languages, as it communicates exclusively through function calls operating on opaque structs, and without transferring ownership of memory. This makes it easy to call using ctypes-style FFI libraries.
* Python - Included in the distribution. Uses ctypes against C API
* Ruby - https://github.com/riboseinc/ruby-botan Uses C API.
* D - https://github.com/etcimon/botan Not actually a language binding but a (third party maintained) fork/rewrite 

### Work In Progress
* Rust (https://github.com/randombit/botan-rs)
* OCaml (https://github.com/randombit/botan-ocaml)

### Now Abandoned
* Perl - Limited wrapper using Perl-XS and the C++ API. Was included in `src/contrib/perl-xs` until 2.4, but was unmaintained.
* Node.js (https://github.com/justinfreitag/node-botan) no updates in many years
