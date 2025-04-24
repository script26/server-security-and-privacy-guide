# Why?
You possibly have a HomeLab (home servers), you want to protect the services running on them, or you are interested in possibly improving the security and privacy on servers.


## Use container focused OS's instead of traditional linux
- They contain an API to setup containers already.
- They usually disallow host shell access (no SSH, unless it's for a container).
- They have extremely minimalist, no debug tools preinstalled for the host (bottlerocket)
- Uses memory safe languages (bottlerocket)
- Kubernetes supported
- Build containers by docker from scratch, to hold only the application needed to run, DO NOT put in debugging tools
- Or as guidance you can use distroless as a build stage in your dockerfile builds
- Possibly try out GVisor

## Or use hypervisors with unikernels
- Instead of containers, you would just run the bare app on a VM, from the hypervisor
- Allows for custom compiled kernels for each unikernel

## Note for both unikernels and containers
- You still need a verified/measured boot implementation to stop OS tampering and malware persistance
 
---

## If you must anyway, here's a traditional linux server setup
- Setup linux repos with HTTPS instead of HTTP.

#### Kernel and procsys changes
- Hardened Linux Kernel (Minimal Amount of Kernel Modules, Hardened Patch, KConfig modifications)
- Hardened Malloc (Run through /etc/ld.so.preload (in musl based distros, use "/usr/lib/libhardened_malloc.so" inside of /etc/ld.so.preload)) (Inspired by GrapheneOS)
- Read Only Partitions (Read only via separated partitions)
- Minimalize kernel leaks of /proc, /sys (hidepid=2)
- Sysctl Minimalized Options

#### Programs, usage and compilation changes
- Compile by hardened flags/minimal libc (PIE, minimalist Libc)
- Reduce Attack Surface (Minimalist kernel modules, minimalist root paritition, minimalize debug tools)
- Reduce Root Access (or even better, NEVER use it)
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
- Sandboxing Applications/Services (gvisor)

#### Usability Removal
- Disable physical logins
- Hide all boot information, in both init and bootloader (typical on linux distros)

#### Usability Addition
- Possibly use Network Bound Disk Encryption (NBDE) to manage decryption of servers remotely/easily (not necessary)
