
 Sometimes a device needs to be opened once at a time; More specifically, do not allow the second open before the release. To implement this restriction, you choose a way to handle an open call for an already open device: it can return an error (-EBUSY), block open calls until a release operation, or shut down the device before do the open.

Let's modify our /dev/msg driver to avoid multiple processes opening the device simultaneously to avoid any concurrency issue.



We will use atomic variables as a solution

Logic:

Initialize the atomic value with 1
In open function, decrement and check whether the value is zero, if zero then return success
If the value is not zero, then increment the value and return EBUSY
In release function, increment the value of atomic variable.
