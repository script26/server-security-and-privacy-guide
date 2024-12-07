## Linux Specific
- Setup alpine linux repos with HTTPS instead of the default HTTP.

---

- Hardened Linux Kernel (Minimal Amount of Kernel Modules, Hardened Patch, KConfig modifications)
- Hardened Malloc (Run through /etc/lib.so.preload)
- Read Only Partitions (Read only via separated partitions)
- Minimalize kernel leaks of /proc, /sys (hidepid=2)
- Sysctl Minimalized Options

---

- Compile by hardened flags/minimal libc (PIE, minimalist Libc)
- Reduce Attack Surface (Minimalist kernel modules, minimalist root paritition, minimalize things you don't need for your particular server)
- Reduce Root Access
- Disable Root Account
- Reduce Processes Running as Root
- Minimalist EFI Partition (Only keep just the EFIStub)
- Secure Boot (Ensure you boot only signed EFIStubs)
- DM-Verity for signed root paritition (Similar to Secure Boot, and continue the RoT - Root of Trust)
- DM-Crypt or FSCrypt for granular encryption
- AppArmor or SELinux for granular access control

---

- Minimalize amount of software/amount of services running on server
- Use minimal init system
- Sandboxing Applications/Services

---

## Usability Removal
- Disable physical logins
