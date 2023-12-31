Capabilities
===============

What is POSIX Capabilities?

Traditionally Linux/Unix only had two level of privileges:
	1. Root
	2. Non-Root

No security checks where performed for processes running in root user, whereas processes running in non-root user were subjected to security checks

No intermediate solution was existing at that time. setuid was only the option for the non-root processes to get privileges. 

Giving all privileges when only few were required was not a good solution and is a target for attack.

POSIX Capabilities is a concept which divides root privileges into a set of privileges.

These privileges/values then can be independently assigned to the processes, by this way the process will only contain the require privileges and some level of security is achieved.

What all Capabilities exist?
==============================

File '/usr/include/linux/capability.h' contains list of capabilities available in Linux

or

man capabilities

Command to find which capabilities are set for a particular file?
===================================================================

getcap <filename>


Command to set capabilities for a particular file?
=====================================================

Each process has three sets of capabilities:
1. Permitted: capabilities that this process can possibly have. Superset of effective
2. Effective:  capabilities that this process actually has.
3. Inheritable: capabilities that this process can pass to a child process

Each capability is implemented as a bit in each of the bitmap, which can be set or unset

$ setcap cap_sys_boot+ep /path/to/executable

The above command sets 'CAP_SYS_BOOT' capabilities in both extended and permitted bitmap


Where are capabilities stored?
==================================

capabilities are implemented on Linux using extended attributes in the security namespace.

All the major file systems such as ext2, ext3, ext4,JFS, XFS etc support extended attributes

What happens when a process tries to perform a privilege operation?
=====================================================================

When a process tries to perform a privileged operation, the kernel checks whether the particular capability for performing the operation is set in the effective capability bitmap, if yes then it allows, else throws 'permission denied' error.

