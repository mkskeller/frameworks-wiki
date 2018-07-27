## What is it?
Obliv-C is an extension of C that implements and executes a garbled circuit protocol.

## Usability
It produces a single independent executable that can be used anywhere, which is convenient.

Some annoyances: 
+ There's some boiler-plate code needed to call the oblivious part.
    * remote host should be set to NULL if you are not the remote host  
    * The examples implicitly cast remote host to a boolean to determine which party is acting.  
    * The function ocTestTcpOrDie is not included in obliv-c proper, but is included in `util.h`, which causes namespace problems  
    * There is a method `cleanupProtocol`. It is not clear why this is needed. Would you ever re-use a ProtocolDesc object?
+ It's possible during the online phase to check which party you are and to do conditional execution based on this fact.

+ It is not clear whether locations in the IO object should be “reused”, or whether some should be allocated for Alice and some to Bob.

## Links
* [paper](https://eprint.iacr.org/2015/1153.pdf)  
* [source code](https://github.com/samee/obliv-c)  