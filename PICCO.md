## What is it?
PICCO is a framework for secure computation with an arbitrary number of parties. It converts an extension of C to native C code, which is compiled and executed by _n_ servers to run a secure computation. It's geared towards cloud computation and separates participants into three groups: input providers, computation servers, and output receivers. 

## Usability
Compile setup isn't exactly correct. We had to remove boost dependency from `picco/compute/objects.mk` (see `picco.patch`).

Compiling is a multi-step affair, and I haven't found an end-to-end example with all then necessary config files. You need to generate public/private key pairs, and set several config files correctly. 
1. compile program to secure c++ program (2 inputs, 2 outputs)  
    `picco <user program> <SMC config> <translated program> <utility config>`
2. create executable (we include makefiles for our examples)  
    `make <example>`
2. generate inputs (automatic secret-sharing) (3 inputs + 1 flag, 1 output arg)  
    `picco-utility -I <input party ID> <input fname> <utility config> <shares fname>`
3. start compilation servers (run this for each server in reverse ID order) (6 inputs, 1 output list)  
    `<example> <ID> <runtime config> <private key> <# input parties> <# output parties> <shares ...> <outputs ...>`
4. start computations (2 inputs)  
    `picco-seed <runtime config> <utility config>`
5. process outputs (3 inputs + 1 flag, 1 output)  
    `picco-utility -O <output party ID> <shares name> <utility config> <result fname>`

## Notes

We did some independent testing of floats (not rigorous and not included in this repository) and had were unable to successfully execute any operations between floating-point numbers. 

Threshold value `t` must be small enough that `2*t+1 <= n` where `n` is the number of parties (this is standard Shamir threshold sharing). This will segfault without comment in `picco-utility`.

We generated keys pairs which are required for secure communication. Our scripts generate 3 pairs by default but this number can be arbitrarily increased.

The extended moving around of files in `install.sh` is for my sanity when running through the multi-step compilation. Basically, shared stuff (generating the executable) is in the main example directory, and server-specific stuff (executing) is in the `secure_server` directory. These examples are all run locally from the same directory, but this could be extrapolated to a real distributed setup by copying the files in the server directory to each server (except for other people's private keys, of course).

## Links  
[github](https://github.com/PICCO-Team/picco)  
[paper](http://www.acsu.buffalo.edu/~mblanton/publications/ccs13.pdf)  
[pointer paper](http://www.acsu.buffalo.edu/~mblanton/publications/tops17.pdf)