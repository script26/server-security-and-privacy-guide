# Why?
You possibly have home servers, you want to protect the services running on them, or you are interested in possibly improving the security and privacy on servers. (This doesn't account for networking security, this is only software security for servers)

Almost all other guides aren't going to give you true security, as they recommend a traditional linux setup. I recommend using container focused OS as it's already hardened and host OS is minimalized to only what is needed for core functions to work.


## Use container focused OS's instead of traditional linux
- They contain an API to setup containers already.
- They usually disallow host shell access (no SSH, unless it's for a container).
- They usually have a MAC already setup and enforced
- Way more specialized use case, thus easier to secure (similar to embedded system vs general-purpose system)
- They have extremely minimalist, no debug tools preinstalled for the host (bottlerocket)
- Uses memory safe languages (bottlerocket)
- Kubernetes supported
- Build containers by docker from scratch, to hold only the application needed to run, DO NOT put in debugging tools
- Or as guidance you can use distroless as a build stage in your dockerfile builds
- Possibly try out GVisor
- Production made OS ready for use
- Usually runs on hypervisor as well (or baremetal)
- With docker, you can specify it to run as read-only, disable executability on specific places 

## Or use a hypervisor with unikernels
- Instead of containers, you would just run the bare app on a VM, from the hypervisor
- Allows for custom compiled kernels for each unikernel

## Note for both unikernels and containers
- You still need a verified/measured boot implementation to stop OS tampering and malware persistance
- Coreboot is a minimal payload UEFI replacement, unfortunately not common on server hardware
 
---

## If you must anyway, here's a traditional linux server setup (not recommended, unless it's from a hosting service and it's your only choice)
- Setup linux repos with HTTPS instead of HTTP.
- Usually runs on hypervisor as well (or baremetal)

#### Kernel and procsys changes
- Hardened Linux Kernel (Minimal Amount of Kernel Modules, Hardened Patch, KConfig modifications)
- Hardened Malloc (Run through /etc/ld.so.preload (in musl based distros, use "/usr/lib/libhardened_malloc.so" inside of /etc/ld.so.preload)) (Inspired by GrapheneOS)
- Read Only Partitions (Read only and separated partitions)
- Compile kernel with address-space layout randomization (ASLR)
- Minimalize kernel leaks of /proc, /sys (hidepid=2)
- Sysctl Minimalized Options

#### Programs, usage and compilation changes
- Compile by hardened flags/minimal libc (PIE, minimalist Libc)
- Reduce Attack Surface (Minimalist kernel modules, minimalist root paritition, no debug tools)
- Reduce Root Access (or even better, NEVER use it)
- Read-only and disable executability wherever possible
- Disable Root Account
- Reduce Processes Running as Root
- Minimalist bootloaders
- Minimalist init system
- Secure Boot (signed EFIs), or Verified Boot/Measured Boot
- DM-Verity/FS-Verity for signed root paritition (Continual Root of Trust)
  - You need to use read only root partition
- DM-Crypt/FS-Crypt
- AppArmor (or SELinux for more granular access control)
- Run only the app that is needed, avoid combining OS tools, services, processes

#### Minimalization
- Minimalize amount of software/amount of services running on server
- Use Docker to isolate services
- Use GVisor to further isolate services by sandboxing

#### Usability Removal
- Disable physical logins
- Hide all boot information, in both init and bootloader (typical on linux distros)

#### Usability Addition
- Possibly use Network Bound Disk Encryption (NBDE) to manage decryption of servers remotely/easily (not necessary)
