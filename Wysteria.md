Wysteria is a mixed mode MPC compiler. It supports unlimited parties.

### What does it do?
High level languages to  circuits, executes using an external MPC library.

### Usability
Documentation is somewhat incomplete. Examples work, but tutorial doesn't (there's a disclaimer). Limited support for some language features described in the paper (in particular, we weren't able to compile code using arrays as a secure input).

The high-level language is new, follows functional programming paradigms (lambda function specification, let/in sequencing). Unfamiliar to those used to working in imperative languages.

Running a computation is still primitive:  
- List out addresses and parties in `addresses.txt`. 
- Start _n_ separate GMW servers
- Start _n_ programs

I found it frustrating to keep several variables consistent across multiple scripts and programs (number of parties, ports used, party names)

Claims to be a true multi-party protocol. Successfully ran a 3-party and a 6-party computation with their `mill.wy` example. Conveniently, the function works for arbitrary parties, I only had to change the setup (declaring parties, starting servers). I haven't gotten an 11-party to work but it's very likely a typo somewhere (all parties are throwing "Fatal error: exception Not_found")

> The WYSTERIA compiler employs _dynamic circuit generation_ to produce circuits when unknowns (like wire bundle lengths) become available.

Circuits are not generated "on-the-fly", but if there are multiple secure blocks in a single program, the later ones don't have to be generated at compile time. For example, you could use the output of one secure block as a list length for a second.

The Choi library doesn't appear to encrypt communication channels.

## Sample Programs

### Multiply 3 Numbers
Generated circuit size: 2473 gates, 67495 byte file (per user; each user gets an identical circuit in this example).

Timing: 0.572-0.588 seconds for Wysteria program, ~0.6 seconds for server (but this also requires some time (~1 sec) to set up).

### Dot product
No working implementation yet. 

### Crosstabs
No working implementation yet.

## Links
- [paper](http://www.cs.umd.edu/~aseem/wysteria-tr.pdf)
- [source](https://bitbucket.org/aseemr/wysteria/wiki/Home)