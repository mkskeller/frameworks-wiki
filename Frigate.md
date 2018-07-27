
### What does it do?
Main contribution is a circuit compiler takes a high-level C-like language to a Boolean circuit. They also include a new compact circuit representation and a "circuit interpreter". 

Frigate strongly emphasizes simplicity and correctness in their compiler design. The language is simple, only defining signed and unsigned integers of arbitrary length and struct types. Since the compiler is not tied to a specific MPC protocol, the circuits it generates can be used in any setting (malicious, semi-honest) that can be executed with a Boolean circuit.

### Usability
Lots of options means the developer has to know something about how to write code that might optimize the circuit (eg input/output int sizes)

Testing the output requires finding an alternate MPC runtime that can execute a Boolean circuit. 

## Sample Programs

### Multiply 3 Numbers
They provide a sample for multiplying 2 numbers together. This appears to use some optimization for multiplication, including defining a smaller int type for inputs and a larger for outputs. Generalizing this to three parties doesn't work, so the results for a 3 party use a single int size. (for k-bit inputs, this means 3*k-bit integers)

Generated circuit size for 256 bit input: 3526674 gates. 60590840 byte circuit written in ascii. I think there's a compressed format but I can't tell which file it is.

Timing: 

### Dot product
No working implementation yet. 

### Crosstabs
See [crosstabs_brute.wir](../blob/master/frigate/crosstabs_brute.wir), [crosstabs_hash.wir](../blob/master/frigate/crosstabs_hash.wir), and [crosstabs_dp.wir](../blob/master/frigate/crosstabs_dp.wir).

Timing is reported by the compiler (maybe we want to double-check this).
We also have the option to report number of non-xor gates.

|  | Brute | Hash | DP |
| ---|----|----|----|
| Generated Circuit Size (total gates) | 2890907 | 115678907 | 148177 |
| Compiler Timing | 0.540819 | 24.8573 | 0.030338 |
| Interpreter Timing | 0.068969 | 8.39161 | 0.004842 |

## Links
- [paper](http://work.debayangupta.com/papers/eurosp16.pdf)
- [source](https://bitbucket.org/bmood/frigaterelease.git)