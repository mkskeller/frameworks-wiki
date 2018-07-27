## What is it?
ObliVM compiles a Java-like language and executes a two-party garbled circuit protocol. It includes a new, independent language ObliVM-lang, and aims to provide an intuitive language for non-experts with several specific programming abstractions for improved performance.

## Usability

Difficulties with ObliVM:
- We couldn't find a way to output more than 32 bits of information
- Only supports providing inputs as a bit array (of ASCII 0s and 1s).
- There are a number of steps needed to compile input
- It is not clear what the @n and @m mean, or what you should do if the inputs are the same length.
- ``alice`` and ``bob`` are reserved words. This is not documented anywhere. 
- The only way to make secret values public is to have them be the return value of a function.
     This means the number of times a loop will iterate cannot depend on parameters of the function (except input length).

We found that limited documentation severely reduced the usability of this framework.

## Links
[paper](https://www.cs.umd.edu/~elaine/docs/oblivm.pdf)  