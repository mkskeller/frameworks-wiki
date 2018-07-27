## What Is It?
SCALE-MAMBA is a framework for true multi-party computation. It is secure in a malicious model (that is, the computation will abort with a malicious adversary) and it supersedes the SPDZ language. It uses an arithmetic model of computations (ie over a large finite field) but the protocol itself uses a hybrid approach (diverging far from the original circuit models).

MAMBA is the front end. It compiles a Python-ish language to a bytecode representation. The bytecodes are fully specified in the documentation (see below). SCALE executes the secure protocol on bytecodes. It uses three distinct types of pre-processing data to execute the secure protocol.

## Usability

This framework has extremely extended documentation in their repository (download it and `make docs`). It's extremely in-depth and covers a lot of ground. Worth several reads, plus re-reading any time you have a question. 

Installation comments:
- MPIR and OpenSSL libraries need to be downloaded and compiled from source. Default versions on your OS won't work (or at least, it didn't for me).
- We needed custom `yum` settings to install one of the required packages (`epel-release`).
- Mini certificate authority was somewhat of a pain to set up. See `install.sh` for our successful version. We generate 4 players by default, but S/M supports arbitrarily many. 

We tested this framework in a relatively limited setting -- isolated, single-execution unit tests -- and were relatively slow, complex, and resource-intensive (on our systems, tests required ~8GB RAM to run). However, it supports a great deal of customization in protocol settings and use cases, and has been used successfully as a component of larger secure systems e.g. the Jana project from Galois. For more discussion on this topic, please query the [SCALE-MAMBA discussion list](https://groups.google.com/forum/#!topic/spdz).

The framework is in active development. Our work is compatible with Version 1.0, the first version that was released. No guarantees for now about V1.1 and beyond.

## Links
- [code](https://github.com/KULeuven-COSIC/SCALE-MAMBA)
- [google group](https://groups.google.com/forum/#!forum/spdz)