
### What does it do?
ABY is a library-style framework for secure computation that provides fine-grained control over computation efficiency via multiple protocol support. 

It supports three sharing schemes: Arithmetic sharing, Boolean sharing, and Yao's garbled circuits. Arithmetic-shared circuits are evaluated using a novel GMW-based protocol, fully described in the paper. Boolean-shared circuits are evaluated using the GMW protocol. Yao circuits are evaluated with Yao's garbled circuits protocol. Users can manually switch between circuit types during the secure computation. 

Function specification is done via gates. There are some useful abstractions (parallelizing concurrent operations with SIMD gates; add, multiply, subtraction, inversion gates for all circuit types, plus bitwise and, xor, or, greater than, equality, and multiplexer gates for Boolean and Yao shares), but the developer is still implementing functions with a fairly low-level representation.

The framework only supports two parties, but you can secret share from multiple parties to two, then rebuild the inputs and implement the function.

### Usability
Compilation was fairly straightforward, although it failed on OSX.

Their extensive user guide explains most of hte basic functionality of the framework and explains how the type system is structured.
It has some omissions (EQ gate for Boolean circuits, ADD for shares or for `wireids`, pre-computation phase used in `min-euclidean-dist` example) and out-of-date entries (`get_wire_ids` does not exist, contradiction between statements in UG and implementations). For example,
> Changing the bitlength only works for Boolean or Yao sharing, but not for arithmetic sharing.

but `innerprod.cpp` calls `set_bitlength` at the end of the computation in an Arithmetic circuit.

Arithmetic circuits define ADD and MUL gates that takes wire ids as input, rather than shares. This is not documented in the User Guide and is not supported for Boolean circuits.

We found some correctness issues. Some are user errors that result in incorrect answers, and some seem to be development errors.

- The same share type is used to represent secret input as understood by each circuit. This is a useful abstraction, but there's no enforcement re: which circuit type a share represents at any given time. For example, you can have an Arithmetic circuit output a share generated on a Yao circuit. This doesn't produce any warnings or errors, and will produce incorrect output. 

- Potential bug converting from Boolean to Arithmetic shares. With more than 4 conversions in a single circuit, `share` value is incorrectly calculated. (See `eq_conversion_failure`).

- Potential bug with Yao: fails to output ADDed shares. Note: this may be an integer overflow error caused by some error in earlier initialization.
```
share *s_x = yaocircuit->PutINGate(10,32,SERVER); // arguments: (value, num_bits, party)
share *s_y = yaocircuit->PutINGate(24,32,CLIENT);
s_x = yaocircuit->PutADDGate(s_x,s_y);
// FAILS
s_x = yaocircuit->PutOUTGate(s_x, ALL);
```
However, if you switch representations to an Arithmetic circuit before the output gate, it works:
```
s_x = arithcircuit->PutY2AGate(s_x, booleancircuit);
s_x = arithcircuit->PutOUTGate(s_x, ALL);
```
(please forgive gratuitous uninitialized circuits).

---
All network communications trace back to the [ENCRYPTO_utils](https://github.com/encryptogroup/ENCRYPTO_utils) socket files. Secure communications shouldn't matter, though, because it's a two-party computation.

ABY uses OpenSSL which uses AES-NI by default unless configured with `no-asm` option. I grepped the repo and didn't find that string, so I strongly suspect this framework does use AES-NI.

## Sample Programs
Method `GetNumGates` on `Circuit` types is not fully implemented, so we're not yet reporting circuit size in gates.

### Multiply 3 Numbers
Implemented.
Since the framework only supports two-party computations, multiplying 3 numbers requires secret-sharing 3 inputs amongst the two parties.

Arithmetic circuits operate in a field of size 2^`l`, where `l` is the bit size of the types computed on. For these examples, we used 16-bit numbers, and the results were given mod 2^16. The largest bit size available is for the `double` type (`l = 64`).

~~Current implementation generates inputs and their shares randomly. It is almost identical to the Dot Product circuit, but with add and multiply gates switched.~~

Using PutSharedInGates automatically adds the shares, but isn't implemented for 32 bit inputs.

Runtime considers the minimum of the two parties.

|   | Compilation (sec)  | Runtime (sec) |
| ------------- |-------------| -----|
| Arithmetic  | 3.77 | 0.35 |
| Boolean | | |



### Dot product
This is one of their existing examples.

Timing for the circuit as-is:

|   | Compilation (sec)  | Runtime (sec) |
| ------------- |-------------| -----|
| Arithmetic  | 3.74 | 0.36 |



### Crosstabs
No working implementation yet. 

Potential errors converting 0/1 gates from Boolean shares to Arithmetic shares.

## Links
- [source](https://github.com/encryptogroup/ABY)
- [paper](http://thomaschneider.de/papers/DSZ15.pdf)
- [user guide](https://www.encrypto.informatik.tu-darmstadt.de/fileadmin/user_upload/Group_ENCRYPTO/code/ABY/devguide.pdf)