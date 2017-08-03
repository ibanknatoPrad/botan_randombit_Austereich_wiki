Though Botan is written in C++ it is possible to use it from some other languages including

* C89 - Available out of the box in the header `ffi.h`. This C interface is also intended to be the preferred way of binding Botan to other languages, as it communicates exclusively through function calls operating on opaque structs, and without transferring ownership of memory. This makes it easy to call using ctypes-style FFI libraries.
* Python - Included in the distribution. Uses ctypes
* Ruby (https://github.com/riboseinc/ruby-botan): Fairly complete.
* Node.js (https://github.com/justinfreitag/node-botan): seemingly abandoned
* OCaml (https://github.com/randombit/botan-ocaml): in a very preliminary state