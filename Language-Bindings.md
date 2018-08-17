Though Botan is written in C++ it is possible to use it from some other languages including

### Ready To Use

* C89 - Available out of the box in the header `ffi.h`. This C interface is also intended to be the preferred way of binding Botan to other languages, as it communicates exclusively through function calls operating on opaque structs, and without transferring ownership of memory. This makes it easy to call using ctypes-style FFI libraries.
* Python - Included in the distribution.
* Ruby - https://github.com/riboseinc/ruby-botan
* Rust - https://github.com/randombit/botan-rs

### Experimental/Work In Progress
* OCaml https://github.com/randombit/botan-ocaml

### Abandoned?
* Adobe AIR (https://github.com/vpmedia/botan-crypto-ane)
* PHP (https://github.com/kisscool-fr/php_botan)
* Perl - Limited wrapper using Perl-XS and the C++ API. Was included in `src/contrib/perl-xs` until 2.4
* Node.js (https://github.com/justinfreitag/node-botan)
