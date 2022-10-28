# Linux_hw1
system_call amd multi-thread  
環境: ubuntu16.04-64bits   

kernel code: linux-4.15.1.tar.gz    
(https://cdn.kernel.org/pub/linux/kernel/v4.x/)  

$ cd linux-4.15.1  


$ mkdir mycall  
$ cd mycall  


$ vim helloworld.c    
#include <linux/kernel.h>  
asmlinkage int sys_helloworld(void) {  
    printk("hello chloe\n");  
    return 1;  
}  

$ vim Makefile  
obj-y := helloworld.o  


$ cd ..  
$ vim Makefile  
core-y += kernel/ mm/ fs/ ipc/ security/ crypto/ block/ mycall/  

$ vim arch/x86/entry/syscalls/syscall_64.tbl    
440     common  identity                sys_identity  

$ vim include/linux/syscalls.h     
asmlinkage int helloworld(void);  

## Kernel Syscall
### 宣告
```c
SYSCALL_DEFINE3(my_get_physical_addresses, unsigned long*, initial, unsigned long*, result, int , n)  
```
> initial = virtual address  
> result = physical address
> int = # of PA elements

