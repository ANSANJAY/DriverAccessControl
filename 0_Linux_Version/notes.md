Writing Modules for Multiple Kernel Versions
==============================================

We have seen function arguments changes between linux kernel versions for example access_ok

Before 5.0 Linux Version.

int access_ok(int type, void *addr, unsigned long size);

After 5.0

int access_ok(void *addr, unsigned long size);


If you want to support multiple kernel versions, you'll find yourself having to code conditional compilation directives

 The way to do this to compare the macro LINUX_VERSION_CODE to the macro KERNEL_VERSION.

LINUX_VERSION_CODE:This macro expands to the binary representation of the  kernel version.
One byte for each part of the version release number.
Eg. 5.0.0 = 0x050000 = 327680

Header File: <linux/version.h>

From Kernel Top Level Makefile

LINUX_VERSION_CODE = $(VERSION) * 65536 + $(PATCHLEVEL) * 256 + $(SUBLEVEL)
Eg: 5.0.0 = 5*65536+0*256+0 = 327680
