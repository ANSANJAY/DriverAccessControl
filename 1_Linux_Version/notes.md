KERNEL_VERSION
===============

This macro is used to build an integer code from the individual numbers that build up a version number.

#define KERNEL_VERSION(a,b,c) (((a) << 16) + ((b) << 8) + (c))

Header File: <linux/version.h>

