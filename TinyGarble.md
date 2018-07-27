## What is it
TinyGarble is a framework for two-party secure computation. It takes a 2-step approach to computation
1. convert a high-level function (defined in Verilog) to a custom circuit description (SCD)
2. execute Yao's garbled circuit protocol

While they have some pre-compiled examples for 1, the main contribution of the source code is 2.

## Usability
The Verilog->SCD step is not well documented. It looks like they did most of their preliminary examples with the proprietary Synopsys Design Compiler, and they have (what appears to be) fairly complete documentation for using this tool. They include brief instructions for using an open-source synthesis tool, Yosys, but don't go into much depth. 

Yosys: the sample instructions aren't documented. It's not clear what the goal of running the commands is, which makes it harder to debug. Tried running them on `hamming.v` and `compare_nbit_ncc.v`. In both steps, the `hierarch` command failed with  
> `ERROR: Module '\COUNT' referenced in module '\hamming' in cell '\COUNT_' is not part of the design.` 
 
The next steps seem to be optimizations (this is fine). The next step includes a library, which wasn't necessary for either of my examples. The final step writes verilog, presumably in the unspecified "netlist" format. Once you have a netlist file, according to the instructions, you
> Translate the netlist file (`.v`) to a simple circuit description file (SCD) using TinyGarble's `V2SCD_Main`

Unfortunately, this step cause a segmentation fault with no error messages.

---
Running the TinyGarble executable produces a vast amount of log files that are invariably empty.

## Sample Programs

### mult3

### innerprod

### xtabs

## Links
- [source](https://github.com/esonghori/TinyGarble)  
- [paper](https://www.ieee-security.org/TC/SP2015/papers-archived/6949a411.pdf)