# ECE695 Modern Data Center Systems

*Guangtong Shen*

*Fall 2016*

### 20. Xen and the Art of Virtualization

#### Summary

Xen is a Virtual Machine Monitor featuring Paravirtualization which has direct hardware access and guest management responsibilities. The guest OS needs to be modified while the applications in the guest system is not modified.

#### Q1 In the Xen paper, it was pointed out that ESX (and full virtualization in general) incurs a lot of performance overhead because of X86 (X86 is hard to virtualize and tricks need to be played to virtualize it). Do you think with other platforms (e.g., MIPS), full virtualization will still have big performance overhead compared to paravirtualization Why and why not

Paravirtualization eliminates the need for the virtual machine to trap privileged instructions.
So long as the CPU has kernel mode privileged instructions, paravirtualization will perform better than full virtualization.

#### Q2 Xen removes the performance cost of TLB miss handling in X86, but it still needs to be involved when the guest OS inserts a page table entry. Will this performance overhead be significant compared to TLB misses. Which is more likely to happen (TLB miss or page table change) and which has higher cost

The hypervisor has a shadow page table which maps virtual address to real machine address. Whenever the mapping changes (eg. new page table entry), the hypervisor captures the Virtual to Physical Address Mapping in the guest OS and creates the shadow page table entry. 
TLB miss is more like to happen. So translating when the entry is inserted into page table has lower cost than tanslating using software on TLB miss .

#### Q3 Both Xen and ESX were designed for a certain type of hardware architecture and made many of design decisions based on that architecture (e.g., deal with hardware-managed TLB, leverage domain 1). And both of them were designed for X86. Do you think this is a good strategy for software (e.g., what about other hardware architecture and what if the hardware architecture change)

Unlike many applications, hypervisors are platform-specific. The meaning of hypervisors are abstraction of hardware and provide indentical software interface for guest OS. When X86 is popular, hypervisors should first meet the need to X86 users. The strategy has no problem.
