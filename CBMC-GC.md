
### What does it do?
CBMC-GC is a compiler that converts "full ANSI C" to a circuit representation. It is based on the C bounded model checker (CBMC), which converts C programs to an immediate representation (GOTOs only) and unrolls loops and recursive function calls. CBMC-GC adds two additional features: generating an intermediate circuit representation and optimizing the circuit by increasing XOR gates while keeping the overall size small.

### Usability
Framework installation was fairly simple.

Some programs need an appropriately set compiler flag to finish compilation.
* `--minimization-time-limit x` determines how long to try minimizing. 
* `--unroll x` gives an upper bound for unrolling loops if it can't be determined by static analysis. Might be big, depending on how you've written your program.

Once I figured out the Makefiles, this was easy to write. 

There's some kind of testing infrastructure built into some of the Makefiles that involves allocating 900k variables that I've never been able to compile. Every C++ compiler/OS combo I've tried chokes. I've removed it from the examples.

Inputs to the main program must be called `INPUT_A`, `INPUT_B`, and so on. They can be an `int` or a `struct`. If you'd like to input an array, you have to wrap it in a struct (see `innerprod/main.c`). Constants, including array lengths, must be `#define`d at the top of the program. Variable array length is not an option.

Compatibility with other frameworks:

ABY compatibility is way out of date with the current ABY code structure. CBMC-GC authors included an ABY example called `aby-cbmc-gc` that should compile to an ABY executable. No success compiling:
- `aby-cbmc-gc` includes don't match current ABY code structure (`/util/` instead of `/ENCRYPTO_utils` and `/ABY_utils/`)
- `aby-cbmc-gc` code requires c++14 features, but ABY compiles by default with c++11
- `aby-cbmc-gc` calls circuit methods and members in CMBC-GC code that don't exist

We produced SCD files but weren't able to run them with TinyGarble. That being said, we weren't able to run anything with TinyGarble. Not clear where the issue here lies.

The [Bristol circuit format](https://homes.esat.kuleuven.be/~nsmart/MPC/) is _not_ compatible with SPDZ or SCALE-MAMBA. This is a circuit description language used in some benchmarking tests aeons ago. None of our frameworks seemed to take this as input, so we didn't test it in practice.

## Sample Programs

### Multiply 3 Numbers
Works. 

### Dot product
Works.

### Crosstabs
We have several versions of this. The brute-force algorithm works, though we may have had to adjust the loop unrolling levels. The hash algorithm didn't compile within the time/loop parameters we tried.

See [crosstabs_brute.wir](../../blob/master/cbmc-gc/test-crosstabs/crosstabs_brute/reference.c) and [crosstabs_hash.wir](../../blob/master/cbmc-gc/test-crosstabs/crosstabs_hash/reference.c).

Also have the option to report non-xor gates and before minimzation numbers.
These tests were run with options `--unroll 100` and `--minimization-time-limit` as noted in headers. 


| | Brute (min=10)  | Brute (min=5)  |
| ------------- |-------------| -----|
| Generated Circuit Size (total gates) |  936907 | 936920  | 
| Compiler Timing (sec)  | 26.1 | 21.6 |


## Links
- [CBMC-GC original webpage](http://forsyte.at/software/cbmc-gc/)
- [v2 webpage](https://www.seceng.informatik.tu-darmstadt.de/research/software/cbmc-gc/)
- [v1 short paper](https://arise.or.at/pubpdf/CBMC-GC__An_ANSI_C_Compiler_for_Secure_Two-Party_Computations.pdf)
- [long paper](https://dl.acm.org/citation.cfm?id=2382278)
- [sample application](https://link.springer.com/chapter/10.1007/978-3-319-47166-2_63)