As you know the open system call takes set of flags as second argument that control opening a file and mode as third argument that specifies permission the permissions of a file if it is created

int open(const char *pathname, int flags, mode_t mode);

The do_sys_open function starts from the call of the build_open_flags function which does some checks that set of the given flags is valid and handles different conditions of flags and mode.

File: fs/open.c

Let's look at the implementation of the build_open_flags. This function is defined in the same kernel file and takes three arguments:
flags - flags that control opening of a file;
mode - permissions for newly created file;
The last argument - op is represented with the open_flags structure:

struct open_flags {
        int open_flag;
        umode_t mode;
        int acc_mode;
        int intent;
        int lookup_flags;
};

which is defined in the fs/internal.h header file and as we may see it holds information about flags and access mode for internal kernel purposes

Implementation of the build_open_flags function starts from the definition of local variables and one of them is:

int acc_mode = ACC_MODE(flags);

This local variable represents access mode and its initial value will be equal to the value of expanded ACC_MODE macro. 

#define ACC_MODE(x) ("\004\002\006\006"[(x)&O_ACCMODE])
#define O_ACCMODE   00000003

The "\004\002\006\006" is an array of four chars:

"\004\002\006\006" == {'\004', '\002', '\006', '\006'}
So, the ACC_MODE macro just expands to the accession to this array by [(x) & O_ACCMODE] index. As we just saw, the O_ACCMODE is 00000003. By applying x & O_ACCMODE we will take the two least significant bits which are represents read, write or read/write access modes:


#define O_RDONLY        00000000
#define O_WRONLY        00000001
#define O_RDWR          00000002

