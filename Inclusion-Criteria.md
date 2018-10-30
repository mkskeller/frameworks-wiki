# Criteria

This project surveys general-purpose compilers for secure multi-party computation. We only include frameworks that meet the following criteria. Questions about these criteria can be raised in the issue tracker.
- *Defines a high-level language*. This should be human read- and write-able (e.g. not a gate-by-gate circuit representation). This can be an independent domain-specific language, an extension of an existing language, or a library defined within an existing language. 
- *Processes the language*. Frameworks should include some compiler component that converts the language to an intermediate representation (e.g. traditionally this is a circuit, but no particular format is required)
- *Executes a protocol*. A runtime component should consume the compiler output and execute a protocol. This should be a full execution, including networking, I/O, and other practical concerns. 
- *Recently maintained*. We only include projects that have been updated since 2014.

We will evaluate compiler-only projects on a case-by-case basis. Please create an issue if you maintain such a framework. 

# Adding your project
If you determine that your project meets the above criteria, you are welcome to nominate it for inclusion by creating a pull request. We request that all PRs include
- a `Dockerfile` and (optional) `install.sh` to set up all necessary dependencies, download and install the framework, and compile example programs
- a `README.md` with the following information: Docker setup (how to set up and run the docker instance), Architecture (a brief overview of the structure of the framework), Running examples (*explicit* instructions to generate input, navigate to the appropriate executable files, set up and run an execution), Modifying examples (*explicit* instructions on how to build examples, including changes to `cmake` or `make` files and compilation steps)
- a `source/` directory containing all necessary files to compile and execute three example problems: 
    - `mult3`: multiply three integers together from 3 separate input parties, return an integer
    - `innerprod`: compute the inner product of two length-10 vectors of integers, return an integer
    - `xtabs`: input is a pair of lists (length 10 or longer). for party A: IDs (integers) and bins (integers [0,5)). for party B: IDs (integers) and values (integers). Computes the bin-wise sum of values for IDs that are in both lists. Return a list (length 5) of sums.

Questions about these items should be raised in the issue tracker.

# Excuses

There are many valuable projects in the field of MPC that we did not include in our survey. We list some of them here. We determined that these projects do not meet the criteria listed above.

- VIFF ([code](http://viff.dk)) 2008, has been "left alive for archival purposes" but they actively recommend against use.
- TASTY ([github](https://github.com/tastyproject/tasty), [paper](https://eprint.iacr.org/2010/365.pdf)) has not been updated since 2010.
- PCF ([paper](https://www.usenix.org/system/files/conference/usenixsecurity13/sec13-paper_kreuter.pdf), [github](https://github.com/cryptouva/pcf), [paper ii](https://dl.acm.org/citation.cfm?id=2517877)) is a circuit generator. It seems to be subsumed by Frigate. The code was last updated in 2016. 
- L1 ([paper](https://eprint.iacr.org/2010/578.pdf)) 2011, SAP does not release source code, so it's not available to study.
- SEPIA ([paper](https://www.usenix.org/legacy/events/sec10/tech/full_papers/Burkhart.pdf)) 2010. This is a special purpose framework for network events.
- EzPC ([paper](https://eprint.iacr.org/2017/1109.pdf)) has not released source code at time of writing.
- FRESCO ([github](https://github.com/aicis/fresco)) This is an API for secure computation. It aims to make frameworks more consistent but doesn't provide an end-to-end framework itself.
- SCAPI ([github](https://github.com/cryptobiu/libscapi)) is an API for primitives, protocols, and communication channels commonly used in MPC. 
- dualBatchEx ([github](https://github.com/osu-crypto/batchDualEx), [paper](https://eprint.iacr.org/2016/632)). The primary contribution is a protocol implementation. It consumes circuits.
- Duplo ([github](https://github.com/AarhusCrypto/DUPLO)) This is primarily a protocol implementation. It uses Frigate as a front-end, which we discuss.
- TinyLego ([github](https://github.com/AarhusCrypto/TinyLEGO)) is primarily a protocol implementation. The front-end is a circuit implementation & parser.
- JustGarble ([website](https://cseweb.ucsd.edu/groups/justgarble/)). This library only implements garbling and evaluation on Boolean circuits. It doesn't include communication or circuit generation. It's mostly subsumed by TinyGarble.
- Semi-honest BMR ([github](https://github.com/cryptobiu/Semi-Honest-BMR)). This is primarily a protocol implementation. It consumes circuits.
- SPDZ has been subsumed by SCALE-MAMBA. We had already reviewed SPDZ when S-M was released, so we include a SPDZ Docker instance and sample programs here for posterity.