Though Botan is written in C++ it is possible to use it from some other languages including

### Working/Functional

* C89 - Available out of the box in the header `ffi.h`. This C interface is also intended to be the preferred way of binding Botan to other languages, as it communicates exclusively through function calls operating on opaque structs, and without transferring ownership of memory. This makes it easy to call using ctypes-style FFI libraries.
* Python - Included in the distribution. Uses ctypes against C API
* Ruby - https://github.com/riboseinc/ruby-botan Uses C API.
* D - https://github.com/etcimon/botan Not actually a language binding but a (third party maintained) fork/rewrite 

### In Progress
* OCaml (https://github.com/randombit/botan-ocaml): in a very preliminary state

### Abandoned
* Perl - Limited wrapper using Perl-XS and the C++ API (included in `src/contrib/perl-xs`). It is now deprecated and likely to be removed in a future release.
* Node.js (https://github.com/justinfreitag/node-botan) no updates in many years
