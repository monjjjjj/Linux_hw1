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

### kmalloc
```c
va = (unsigned long*)kmalloc(sizeof(unsigned long)*n,GFP_KERNEL);
pa = (unsigned long*)kmalloc(sizeof(unsigned long)*n,GFP_KERNEL); 
```
>kmalloc is the normal method of allocating memory for objects smaller than page size in the kernel.  
>GFP_KERNEL: Allocate normal kernel ram.


### kfree
```c
void kfree(const void *objp)
```
>objp：內存位址，通常是kmalloc( )函数的返回值，即是指向分配的內存區塊起始位址的位址指针  

### PID of process都是一樣的?
>在kernel中，每個thread都有自己的ID，可以稱它為PID或者TID，而這些thread同時也有一個TGID(thread group ID)，第一個thread的PID會被當作TGID。

### Thread
>All threads belonging to the same process share code section, data section, and OS resources (e.g. open files and signals).  
>But each thread has its own thread ID, program counter, register set, and a stack (會放在thread control block裡).  

```c
pthread_create()
```
>透過 pthread_create () 建立 child thread 並指定其所要執行的函數，main thread 與 child thread 將會平行執行。
```c
pthread_join()
```
>在 main thread 中使用 pthread_join () 來等待 child thread 結束。  
>若沒有等待child thread 結束， main thread 就直接結束的話， child thread 將無法成功執行完。
