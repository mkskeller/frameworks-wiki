# A Layperson’s Guide to General Purpose Compilers for Secure Multi-party Computation

This article presents our recent Systemization of Knowledge paper for a general audience. Our work surveys 11 frameworks for secure multi-party computation (MPC) and evaluates them on language expressibility, cryptographic abilities, and developer usability. We also provide a complete runtime environment via a Docker image for each framework which installs dependencies, compiles the frameworks, and sets up our three sample programs for testing.

## What is MPC?
Secure multi-party computation (MPC, but alternately known as SMC) is a mechanism by which a group of data owners can compute joint functions on their private data without revealing anything about their underlying data except what is revealed by the output of the computation. It essentially provides a cryptographic version of a trusted third party. 

The classic motivating example is the millionaire’s problem: you (a millionaire) are eating dinner with your wealthy friends. You’d like the richest person to pay for the meal, but none of you wish to reveal your net worth. Rather than secretly telling your net worth to the waiter, the diners can execute an MPC protocol to determine who is the wealthiest without revealing their sensitive financial information to each other or any outside party.

## Why do I care?
In practice, MPC is a valuable tool for any group of shareholders who have some mutual distrust. Potential use-cases for MPC abound, and prototype systems have been developed to address a variety of real-world use-cases, including:
- **Preventing satellite collisions**: Satellite companies do not wish to reveal the exact locations of their on-orbit assets, but also don’t want mid-space collisions with other satellites.  A secure MPC protocol between companies could [determine the probability of collision](https://www.scientificamerican.com/article/cryptographers-could-prevent-satellite-collisions/) without revealing any of the underlying orbital information. 
- **Secure auctions**: A group of buyers and sellers [calculate market-clearing prices](https://www.cs.purdue.edu/homes/aliaga/cs197-10/papers/bogetoft.pdf) without revealing sensitive data (such as crop yields or bid values) to other parties, including the auctioneer 
- **Financial analytics**: Companies wish to participate in an aggregated statistical measurement of financial information without revealing sensitive business information. 
This has been used to [identify tax fraud]( https://sharemind.cyber.ee/tax-vat-fraud/) 
and [compute financial statistics](https://www.boston.gov/sites/default/files/document-file-01-2018/bwwc_2017_report.pdf)
- **Privacy-preserving genomic analyses**: Doctors and researchers wish to perform large-scale analysis of human genomes without revealing the sensitive underlying data. The [iDASH competition](http://www.humangenomeprivacy.org/2018/) tests secure computation methods in a biomedical setting; and academic work in this area has demonstrated [optimized protocols](http://cb.csail.mit.edu/cb/secure-gwas/) for large-scale data, [secure infrastructure design](https://academic.oup.com/bioinformatics/article/29/7/886/253610) in a medical context, and [practical secure diagnosis](http://science.sciencemag.org/content/357/6352/692).
- **Transparency in higher education**: The bipartisan “[Student Right to Know Before You Go Act](https://er.educause.edu/blogs/2017/12/student-right-to-know-before-you-go-bill-introduced)”, proposed in 2017 explicitly calls for MPC technology to increase government transparency.

## Why isn’t everyone using MPC?
Despite its potential, MPC has not been widely deployed.  Until recently, many of the MPC protocols were considered too inefficient (in terms of computational and network requirements) for practical applications.  Algorithmic advances coupled with steadily increasing hardware performance has made MPC efficient enough for many (moderate-scale) computations.

Although current MPC protocols are surprisingly efficient, there are still many barriers to implementing and deploying MPC in real-world applications.  Knowledge remains still a barrier, as many people outside of the cryptographic community are not aware of the existence of MPC, and even stakeholders who are aware of the potential benefits of MPC may find it difficult to translate their needs and security requirements into a formal computation that can be securely executed by a cryptographic protocol.  Finally, implementing and deploying explicit MPC protocols is extremely challenging and requires a high level of cryptographic expertise.

In order to facilitate the adoption of MPC, a number of MPC software frameworks have been developed that are designed to help translate real-world requirements into a concrete MPC protocol implementation, ready for execution.  [IARPA’s HECTOR program](https://www.iarpa.gov/index.php/research-programs/hector) is aimed at further improving usability, and we expect to see rapid advances in MPC compiler technology in the coming years. 

## What do frameworks do?
At a high level, frameworks for MPC need to (1) compile a high-level function description to an intermediate representation and (2) execute an MPC protocol on the intermediate representation. Let’s explore the challenges faced in implementing these steps.

First, each framework needs to define a high-level language for consumption. Most MPC protocols operate over only a few operations on specific data types. Modern and effective MPC frameworks include a compiler, which converts a high-level, human-readable language to the primitive operations consumed by the protocol. The frameworks we studied approach this task in a variety of ways: either by writing a library to define secure types; extending an existing language to include such types; or defining a completely new language.

A key challenge for compiler developers is finding a balance between the cryptographic requirements of the underlying protocols and the usability of the framework. While the compiler abstracts away many details of the cryptographic protocol, too much abstraction can result in less efficient MPC executions. When the user has more control over low-level operations, the compiler may be harder to use, particularly for developers with less cryptographic background.

Second, a runtime component needs to implement an MPC protocol. In practice, a runtime sets up communication between computation parties and performs the computation by sequentially executing the appropriate protocol for each primitive operation. Communication channels sometimes must be secure (e.g. via TLS), and this may require setting up some type of public-key infrastructure for the duration of the computation.

For practitioners, selection of an MPC framework should be motivated by the expected function and needs that you’ll have. We define three protocol families that can provide context for general performance expectations (for instance, protocols that use secret-shared data are often more efficient for arithmetic operations, while Boolean representations may be better at comparison and bitwise operations). Every framework can implement arbitrary functions, but a basic understanding of the advantages and limitations of each protocol family can provide valuable context when selecting a framework. We delegate an interested reader to our full paper and the references therein for a more thorough introduction to these trade-offs.

## What did we do?
We tested 11 frameworks for secure multi-party computation and evaluated them on a range of criteria. These are large, complicated systems with non-trivial build requirements and dependencies. We created Docker instances for each framework; this simplifies the setup and build process and will allow future researchers and developers to spend time evaluating frameworks rather than configuring them.

In order to evaluate criteria like usability, documentation, and expressibility, we chose three sample programs that we implemented for each framework. These were 
Multiply 3: multiplies three numbers together
Inner product: take the inner product of two vectors
Crosstabs: computes a database join of sums by category
Together, these tested the ability to take integers and arrays as input, compute basic arithmetic (addition and multiplication), access and operate over arrays, and execute conditionals.

We provide concrete evaluation of the frameworks across three broad categories. 
Usability: Documentation, online resources, and example code
Functionality: Support for various data types, operators, and control structures
Implementation: Special features, architecture, computation models, and I/O specifications
We did not include comparative performance evaluation for several reasons. First, theoretical efficiency metrics don’t translate easily to practical MPC architectures, and we wanted to avoid a misleading comparison between different models. Second, our sample programs don’t represent a practical use case for these frameworks, which will typically be used as a part of a larger system. We found that a realistic testing framework is outside the scope of this work.

For a more thorough explanation of our criteria and results, further details on our experience with the frameworks, individual recommendations, and further discussion, we encourage you to read our full paper. We look forward to cultivating this repository as a useful tool for using and comparing MPC frameworks, and to the continued adoption and application of MPC as a practical tool for improved privacy.
