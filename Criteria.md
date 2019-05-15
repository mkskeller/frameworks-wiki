This page contains updated versions of the tables in the paper. For full explanations of criteria and "partial credit", see the paper.

- Y: yes, supported
- N: no, not supported
- P: partially supported, see paper
- -: not applicable

# Basic Features
The protocol families include Garbled Circuits (GC), Multi-party Circuit-based protocols (MC), and Hybrid protocols.
Threat models are semi-honest, where all parties execute the computation correctly but will try to learn additional information, and malicious, where parties may deviate from the protocol. In all cases, security against a malicious adversary is with abort: a malicious party cannot force an incorrect answer, but can prevent an answer altogether.

| | Protocol family | Parties | Mixed-mode | Semi-honest | Malicious
|---|---|---|---|---|---
EMP-toolkit | GC | 2 |  Y | Y | Y 
Obliv-C | GC |2 | Y | Y | N
ObliVM | GC | 2 | Y | Y | N
TinyGarble | GC | 2 | N | Y | N
Wysteria | MC | 2+ | Y | Y | N
ABY | GC, MC | 2 | Y | Y | N
SCALE-MAMBA | Hybrid | 2+ | Y | Y | Y
Sharemind | Hybrid | 3 | Y | Y | N
PICCO | Hybrid | 3+ | Y | Y | N
Frigate | - | 2+ | N | - | -
CBMC-GC | - | 2+ | N | - | -


#  Documentation
Example code is code that demonstrates end-to-end execution of a computation. Partial support means we needed additional files or tools to run examples.
Example documentation provides context for example code, either as comments in the code or as a separate document. Partial support is explained for individual frameworks in the paper.
Open source frameworks are typically supported by a standard GNU or BSD license. Partial support means closed-source tools or code are required for full functionality. 
An asterisk by last major update means that there has probably been one since writing this down. Feel free to notify us of updates using the issue tracker!

| | Language docs | Online support | Example code | Example docs | Open Source | Last major update|
|---|---|---|---|---|---|---|
EMP-toolkit | N | N | Y | N | Y | 9/2018*
Obliv-C | Y | N | Y | P | Y | 6/2018
ObliVM | N | N | Y | N | Y | 2/2016
TinyGarble | N | N | P | N | P | 10/2018
Wysteria | P | N | Y | P | Y | 10/2014
ABY | Y | N | Y | Y | Y | 10/2018
SCALE-MAMBA | Y | Y | Y | P | Y | 10/2018* 
Sharemind | Y | Y | Y | Y | P | 9/2018*
PICCO | Y | N | P | N | Y | 10/2017* 
Frigate | Y | N | Y | N | Y | 5/2016
CBMC-GC | P | N | Y | N | Y | 5/2017