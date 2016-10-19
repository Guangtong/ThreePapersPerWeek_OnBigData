# ECE695 Modern Data Center Systems

*Guangtong Shen*

*Fall 2016*

## 19. Memory Resource Management in VMware ESX Server (OSDI'02)

#### Summary

ESX Server is a virtual machine hypervisor that runs on hardware rather than host OS.
ESX Server support overcommitment of memory so that the physical memory are more fully used. It needs to reclaim low value pages from guest OS and reallocate the memory.
To further enhance the utilization of memory, ESX Server support memory sharing.

Q1: Because of memory overcommitment, ESX has to design mechanisms to reclaim memory space from VMs (None of the techniques in Section 3 is needed if memory is not overcommitted). What's the benefit of memory overcommitment?

With this layer of memory virtualization, the total allocated memory of all running virtual machines can exceed the total physical memory.
In this way, memories are dynamically allocated so that free memory of a guest OS can be allocated to use by other guest OS. This is very cost effective in memory usage.


Q2: Ballooning requires the installation of a pseudo-device driver or kernel service in the guest OS. Having a device driver or kernel service in the guest OS allows the communication of guest OS and the hypervisor. But does this defeat the goal of unmodified guest OSes? Is there any other way that doesn't require any guest OS and hypervisor communication (and thus no driver is needed)? Why so or why is this hard?

The guest OS is not modified. Instead, it is just installed with some application.
The pseudo-device driver is a good solution to mark the memory space inside guest OS that are reclaimed by the hypervisor.
I found no other way for the guest OS to know where its memory is reclaimed without communication with the hypervisor.


Q3: ESX employs content-based memory sharing. Memory sharing has shown to be very efficient in saving memory space for VMs. The same idea could be used to share memory across processes on a normal OS (no virtualization), but is rarely used. Why isn't content-based memory sharing across processes not as effective as VMs?

VMs share a lot same memory content since they may have same OS and same application running, so memory sharing is useful.
Processes inside a single OS share libraries and kernel services in a memory area managed by the OS. Other than that, contents of each process are usually not the same. So content-based memory sharing is not needed.


