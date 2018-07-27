## What does it do
A high-level language (SecreC 2) compiles to an intermediate representation made up of various macros (oblivious shuffle, multiplication, floating point operations, etc.). Runs a 3-party additive secure computation.

There are at least 3 available versions of the software. The public SDK is only for the high-level language, and does not include any cryptographic back-end. The academic license has a slower back-end. The enterprise version has a formal verification of the intermediate representation and might be a little better. Experimental but non-public versions include other secure computation protocols, like a 2-party additive, maybe Yao's garbled circuits.

The academic version we've received has extended documentation (link below). There is built-in support for pointwise array operations (SIMD). It has a strong, static typing system, including the aforementioned array support, and all primitive data is private.

The current setup is compatible with the open source 2017.12 release (we had new linking errors with 2018.06) and the 2018.03 academic license release (2018.06 changes client code formatting to some degree).

## Usability
Requires an academic/enterprise license to get access. 

Extensive documentation for secrec language, less for c boilerplate necessary to make client programs run.

Built-in support for floats.

Errors are informative, usually.

The open-source Sharemind SDK is really under-documented (especially compared to everything else). I compiled it successfully but there's no docs re: what compilation actually produces. Found a couple executables but they don't have man pages, dunno what to pass to them, etc.

## Architecture
Two programs need to be separately compiled and run: a C file that passes in values, and a `sc` file that defines the secure computation.

## Sample Programs

### Mult3
Implemented.

### Inner Product
This is one of their examples included with the software package.

### Crosstabs
Implemented.

## Links
[Jaak's thesis](http://dspace.ut.ee/handle/10062/56298)  
[Sharemind paper](https://eprint.iacr.org/2008/289.pdf)  
[online docs](http://sharemind-sdk.github.io/stdlib/reference/pages.html)