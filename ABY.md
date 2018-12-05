
ABY is a library-style framework for secure computation that provides fine-grained control over computation efficiency via multiple protocol support. 

It supports three sharing schemes: Arithmetic sharing, Boolean sharing, and Yao's garbled circuits. Arithmetic-shared circuits are evaluated using a novel GMW-based protocol, fully described in the paper. Boolean-shared circuits are evaluated using the GMW protocol. Yao circuits are evaluated with Yao's garbled circuits protocol. Users can manually switch between circuit types during the secure computation. 

Function specification is done via gates. There are some useful abstractions (parallelizing concurrent operations with SIMD gates; add, multiply, subtraction, inversion gates for all circuit types, plus bitwise and, xor, or, greater than, equality, and multiplexer gates for Boolean and Yao shares), but the developer is still implementing functions with a fairly low-level representation.

The framework only supports two parties, but you can secret share from multiple parties to two, then rebuild the inputs and implement the function.

## Usability
Compilation was fairly straightforward, although it failed on OSX.

Their extensive user guide explains most of hte basic functionality of the framework and explains how the type system is structured.
It has some omissions (EQ gate for Boolean circuits, ADD for shares or for `wireids`, pre-computation phase used in `min-euclidean-dist` example) and out-of-date entries (`get_wire_ids` does not exist, contradiction between statements in UG and implementations). For example,
> Changing the bitlength only works for Boolean or Yao sharing, but not for arithmetic sharing.

but `innerprod.cpp` calls `set_bitlength` at the end of the computation in an Arithmetic circuit.

Arithmetic circuits define ADD and MUL gates that takes wire ids as input, rather than shares. This is not documented in the User Guide and is not supported for Boolean circuits.

We found some correctness issues. We contacted the ABY team about underlying implementation issues and these have since been fixed, so we don't discuss them here. We also found an easy user error that can cause incorrect results:

- The same share type is used to represent secret input as understood by each circuit. This is a useful abstraction, but there's no enforcement re: which circuit type a share represents at any given time. For example, you can have an Arithmetic circuit output a share generated on a Yao circuit. This doesn't produce any warnings or errors, and will produce incorrect output. 


All network communications trace back to the [ENCRYPTO_utils](https://github.com/encryptogroup/ENCRYPTO_utils) socket files. Secure communications shouldn't matter, though, because it's a two-party computation.

ABY uses OpenSSL which uses AES-NI by default unless configured with `no-asm` option. I grepped the repo and didn't find that string, so I strongly suspect this framework does use AES-NI.

## Related Work
ABY demonstrates a less seamless approach to hybrid computation, allowing the user fine-grained control via explicit protocol selection. It has since spawned many follow-up works, including several which automate protocol selection between the three options. These include:
- [EzPC](https://www.microsoft.com/en-us/research/uploads/prod/2018/09/ezpceprint.pdf): ABY but with automatic protocol selection and language guarantees. (Follow-up application: [SecureNN](https://www.microsoft.com/en-us/research/uploads/prod/2018/09/securenneprint.pdf), see also MiniONN below)
- [ABY^3](https://eprint.iacr.org/2018/403.pdf): 3-party malicious security, switches between protocols, designed for machine-learning applications. 
- [HyCC](https://dl.acm.org/citation.cfm?doid=3243734.3243786): more automation!

The system has been used in a variety of practical applications
- [Ciphers for MPC and FHE](https://eprint.iacr.org/2016/687.pdf): Implements symmetric-key primitives
- The SIXPACK system [[ANRW16](https://irtf.org/anrw/2016/anrw16-final5.pdf), [CDCSS17](https://mcanini.github.io/papers/sixpack.conext17.pdf)]: Computing routing information at internet exchange points
- [Private set intersection for unequal size sets with mobile applications](https://eprint.iacr.org/2017/670.pdf): Compares PSI protocols in different settings
- [Efficient circuit-based PSI via cuckoo hashing](https://eprint.iacr.org/2018/120.pdf): Implements new circuit-based PSI protocols
- [Oblivious neural network predictions via MiniONN transformations](https://eprint.iacr.org/2017/452.pdf) [[source](https://github.com/SSGAalto/minionn)]: Transforms neural nets to oblivious neural nets without changing model training.
- [Privacy-preserving whole-genome variant queries](https://thomaschneider.de/papers/DHSS17.pdf): Outsourcing queries on genomic data using MPC

## Sample Programs
Method `GetNumGates` on `Circuit` types is not fully implemented, so we're not yet reporting circuit size in gates.

_mult3_:
Since the framework only supports two-party computations, multiplying 3 numbers requires secret-sharing 3 inputs between the two parties.

Arithmetic circuits operate in a field of size 2^`l`, where `l` is the bit size of the types computed on. For these examples, we used 16-bit numbers, and the results were given mod 2^16. The largest bit size available is for the `double` type (`l = 64`).

Using PutSharedInGates automatically adds the shares, but isn't implemented for 32 bit inputs.


ABY includes the inner product as an example. Our version has the same function implementation but is modified to read data from a file.


## Links
- [source](https://github.com/encryptogroup/ABY)
- [paper](http://thomaschneider.de/papers/DSZ15.pdf)
- [user guide](https://www.encrypto.informatik.tu-darmstadt.de/fileadmin/user_upload/Group_ENCRYPTO/code/ABY/devguide.pdf)