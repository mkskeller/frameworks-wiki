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
Obliv-C     | GC | 2 | Y | Y | N
ObliVM      | GC | 2 | Y | Y | N
TinyGarble  | GC | 2 | N | Y | N
Wysteria    | MC | 2+| Y | Y | N
ABY     | GC, MC | 2 | Y | Y | N
SCALE-MAMBA | Hybrid | 2+ | Y | Y | Y
Sharemind   | Hybrid | 3 | Y | Y | N
PICCO       | Hybrid | 3+ | Y | Y | N
Frigate     | - | 2+ | N | - | -
CBMC-GC     | - | 2+ | N | - | -


#  Documentation
Example code is code that demonstrates end-to-end execution of a computation. Partial support means we needed additional files or tools to run examples.
Example documentation provides context for example code, either as comments in the code or as a separate document. Partial support is explained for individual frameworks in the paper.
Open source frameworks are typically supported by a standard GNU or BSD license. Partial support means closed-source tools or code are required for full functionality. 
An asterisk by last major update means that the framework is actively developed or maintained, so this date may already be outdated. Feel free to notify us of updates using the issue tracker!

| | Language docs | Online support | Example code | Example docs | Open Source | Last major update|
|---|---|---|---|---|---|---|
EMP-toolkit | N | N | Y | N | Y | 5/2019*
Obliv-C     | Y | N | Y | P | Y | 2/2019
ObliVM      | N | N | Y | N | Y | 2/2016
TinyGarble  | N | N | P | N | P | 10/2018
Wysteria    | P | N | Y | P | Y | 10/2014
ABY         | Y | N | Y | Y | Y | 5/2019*
SCALE-MAMBA | Y | Y | Y | P | Y | 1/2019* 
Sharemind   | Y | Y | Y | Y | P | 3/2019*
PICCO       | Y | N | P | N | Y | 10/2017 
Frigate     | Y | N | Y | N | Y | 5/2016
CBMC-GC     | P | N | Y | N | Y | 5/2017

# Functionality
We tested functionality through our sample programs. Anything not in the sample programs is based on the author's claims. Will demote things to partial support if people try it and find it lacking (ie floating point operations).

## Data Types
Most partial support is explained for individual frameworks in Section VI of the paper. Updates will be recorded here.
- We tried implementing floating points in PICCO and found the operations supported are limited.
- We couldn't run TinyGarble programs because they require closed-source tools.

| | Boolean |Fixed int | Arbitrary int | Float | Array |Dynamic array | Struct |
|---|---|---|---|---|---|---|---|
EMP-toolkit | Y | Y | Y | Y | Y | Y | Y |
Obliv-C     | Y | Y | N | Y | Y | Y | Y |
ObliVM      | N | Y | Y | Y | P | Y | P |
TinyGarble  | - | - | - | - | - | - | - |
Wysteria    | Y | Y | N | N | P | - | Y |
ABY         | P | Y | N | P | Y | N | Y |
SCALE-MAMBA | N | Y | P | Y | Y | N | P |
Sharemind   | Y | Y | N | Y | Y | Y | Y |
PICCO       | P | Y | Y | P | Y | Y | Y |
Frigate     | N | Y | Y | N | Y | N | Y |
CBMC-GC     | Y | Y | N | P | Y | N | Y |

## Operators
We've only tested these on integers.

| | Logical | Comparisons | Addition | Multiplication | Division | Bit-shift | Bitwise | 
|---        |---|---|---|---|---|---|---|
EMP-toolkit | Y | Y | Y | Y | Y | Y | Y 
Obliv-C     | Y | Y | Y | Y | Y | Y | Y 
ObliVM      | Y | Y | Y | Y | Y | Y | Y 
TinyGarble  | - | - | - | - | - | - | - 
Wysteria    | N | Y | Y | Y | P | N | N
ABY         | Y | Y | Y | Y | N | N | N 
SCALE-MAMBA | N | Y | Y | Y | Y | Y | Y  
Sharemind   | Y | Y | Y | Y | Y | Y | Y 
PICCO       | Y | Y | Y | Y | Y | Y | Y 
Frigate     | N | Y | Y | Y | Y | Y | Y 
CBMC-GC     | Y | Y | Y | Y | Y | Y | Y 

## Grammar
Arrays can be accessed either with a public index ("array access") or a private one. Private index can be implemented natively as a linear-time multiplexer (mux), natively with ORAM (ORAM), or using an ORAM library (lib).

| | Conditional | Array access | Private Index | 
|---|---|---|---
EMP-toolkit | N | Y | N 
Obliv-C     | Y | Y| Lib
ObliVM      | Y | Y | ORAM 
TinyGarble  | - | - | -
Wysteria    | Y | N | N 
ABY         | Y | P | N
SCALE-MAMBA | Y | Y | ORAM
Sharemind   | Y | Y | N
PICCO       | Y | Y | Mux
Frigate     | Y | Y | Mux
CBMC-GC     | Y | Y | Mux

# Architecture and Implementation
These things probably won't change.

## Architecture
We identify three architecture types. Independent languages have their own parser and compilers. Language extensions add features or keywords to existing languages. Libraries are code modules written in an existing language.

| | Dev language | AES-NI | Independent | Extension | Library | 
|---|---|---|---|---|---|
EMP-toolkit | C++      | Y | N | N | Y
Obliv-C     | OCaml, C | Y | N | Y: C |N
ObliVM      | Java     | Y | Y | N | N
TinyGarble  | C/C++    | Y | N | Y: Verilog | N
Wysteria    | OCaml    | - | Y | N | N
ABY         | C++      | Y | N | N | Y
SCALE-MAMBA | Python, C++ | - | Y | N | N
Sharemind   | C/C++    | - | Y | N | N 
PICCO       | C/C++    | - | N | Y: C | N
Frigate     | C++      | - | Y | N | N
CBMC-GC     | C++      | - | N | Y: C | N

## Representation and I/O
Data representation is typically either arithmetic (e.g. shares over a field) or Boolean. Garbled circuit implementations can run on-the-fly (OTF): that is, start execution before the circuit is fully generated.

Some frameworks require input in a specific format, while others allow arbitrary formatting. Some frameworks allow different input types from each party. Array output gets partial support if elements must be returned one at a time. Some frameworks allow a party to receive more than one output values in a single computation.

| | Arithmetic | Boolean | OTF | Arbitrary format | Different input | Array output | Multiple output 
|---|---|---|---|---|---|---|---
EMP-toolkit | N | Y | Y | Y | Y | P | Y
Obliv-C     | N | Y | Y | Y | Y | Y | Y 
ObliVM      | N | Y | Y | N | Y | N | N 
TinyGarble  | N | Y | Y | - | - | - | - 
Wysteria    | N | Y | - | N | N | - | P
ABY         | Y | Y | N | Y | Y | Y | Y 
SCALE-MAMBA | Y | N | - | Y | Y | P | Y 
Sharemind   | Y | N | - | Y | Y | Y | Y
PICCO       | Y | N | - | N | Y | Y | Y
Frigate     | N | Y | N | - | Y | Y | P
CBMC-GC     | N | Y | N | - | Y | Y | P