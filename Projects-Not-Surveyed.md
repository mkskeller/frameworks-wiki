There are many, many projects in the field of MPC that we did not include in our survey. We list some of them here. We determined that these projects do not implement a general-purpose end-to-end MPC framework.

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