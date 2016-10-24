# ECE695 Modern Data Center Systems

*Guangtong Shen*

*Fall 2016*

### 21. A Comparison of Software and Hardware Techniques for x86 Virtualization (ASPLOS'06)

#### Summary

Before x86 supported classical trap-and-emulate virtualization, the work-around solution of virtualization on x86 is Binary Translation. Later x86 changes to permit classical virtualization. The paper compared the two way of x86 virtualization and found that the hardware supported way ofter suffers lower performance than the pure software way. The reason came from the lack of MMU virtualization support from the hardware and inability to coexist with existing software techniques.

#### Q1: Without hardware virtualization assistance (i.e., with traditional X86 architecture) and without paravirtualization (changing guest OS source), is there any other way to handle all X86's privileged instructions in guest OS'es other than binary translation? Can classical "trap-and-emulate" always work? Why? 

As the paper said, the guests running on an interpreter instead of directly on a physical CPU can have privileged instructions interpreted so that no lack-of-trap problems. 

The problems of trap-and-emulate on x86 came from two aspects: visibility of privileged state by the guest and lack of traps when some privileged instructions run at user-level.  

#### Q2: How does hardware VMM make system calls run fast (i.e., how does it win over the software approach)?

The guest OS runs at its intended privilege level (ring 0), and that the VMM is running at a new ring with an even higher privilege level (Ring -1, or "Root mode"). System calls do not automatically result in VMM interventions: as long as system calls do not involve critical instructions, the guest OS can provide kernel services to the user applications. That is a big plus for hardware virtualization.

#### Q3: With hardware MMU virtualization assistance (e.g., Intel EPT), how many memory accesses are needed when the guest OS has 3-level page table and the VMM has 3-level page table? What about we have nested virtualization (i.e., we run a VMM on bare-metal hardware, then run a VMM in the guest OS on top of the first VMM, and then run a guest OS on top of the second VMM; this is supported by many hypervisors); how many memory accesses are needed if the guest OS has 3-level page table and both VMMs have 3-level page tables?

In a TLB miss, each search of one level of page entry in the guest OS leads to all levels of search in the VMM.
So for 3 level page table in both guest OS and VMM, one TLB miss needs 9 access to memory.

In a "VMM-Guest-VMM-Guest" architecture, one TLB miss of the inner Guest OS needs 3^4 = 81 memory access.
