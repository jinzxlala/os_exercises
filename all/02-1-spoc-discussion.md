#lec 3 SPOC Discussion

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 1. 描述PXE的大致启动流程。

## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。
 1. 了解GRUB的启动流程。
 1. 比较NTLDR和GRUB的功能有差异。
 1. 了解u-boot的功能。

## 3.3 中断、异常和系统调用比较
 1. 举例说明Linux中有哪些中断，哪些异常？
 1. Linux的系统调用有哪些？大致的功能分类有哪些？  (w2l1)

<<<<<<< HEAD
 > ANSWER:linux的系统调用大致有190到300多个 分为以下几个类型：
 一、进程控制：fork clone exit pause wait等
 二、文件控制：open creat close write read flock等
 三、系统控制：ioctl acct outb reboot time uname create module等
 四、内存管理：brk sbrk mlock munlock mmap munmap sync cacheflush等
 五、网络管理：getdomainname setdomainname gethostid sethostid等
 六、socket控制：socketcall socket bind connect accept send recv等
 七、用户管理：getuid setuid getgroups setgroups等
 八、进程间通信：sigaction sigprocmask kill msgctl msgget pipe semop等
 关于linux系统调用列表可以参考网址：http://www.ibm.com/developerworks/cn/linux/kernel/syscall/part1/appendix.html

=======
>>>>>>> 837f2fcd06d5f9bbb6a41dc6f55053e08b3ffcf5
```
  + 采分点：说明了Linux的大致数量（上百个），说明了Linux系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
 1. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？(w2l1)
<<<<<<< HEAD
 >ucore的系统调用有22个。大致分类如下：
 一、进程控制：SYS_exit, SYS_fork, SYS_wait, SYS_exec, SYS_yield, SYS_kill, SYS_sleep, SYS_gettime
 二、文件管理：SYS_open, SYS_close, SYS_write, SYS_read, SYS_seek, SYS_fstat, SYS_getdirentry
 三、内存管理：SYS_fsync, SYS_getcwd, SYS_dup
=======
>>>>>>> 837f2fcd06d5f9bbb6a41dc6f55053e08b3ffcf5
 
 ```
  + 采分点：说明了ucore的大致数量（二十几个），说明了ucore系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(w2l1)
<<<<<<< HEAD
 >nm：列出目标文件.o符号清单
 objdump：查看文件的内容信息
 file：检测文件类型
=======
 
>>>>>>> 837f2fcd06d5f9bbb6a41dc6f55053e08b3ffcf5

 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 
 ```
 
 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(w2l1)
<<<<<<< HEAD
 >strace常用来跟踪进程执行时的系统调用和所接受的信号
 调用过程为：
    mmap>>mprotect>>write>>munmap>>open>>access>>fstat>>read>>execve>>close>>brk>>arch_prctl
=======
 
>>>>>>> 837f2fcd06d5f9bbb6a41dc6f55053e08b3ffcf5

 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 1. ucore的系统调用中返回结果的传递代码分析。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
<<<<<<< HEAD
 1. 以ucore lab8的answer为例，尝试修改并运行代码，分析ucore应用的系统调用执行过程。
=======
 1. 以ucore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。
>>>>>>> 837f2fcd06d5f9bbb6a41dc6f55053e08b3ffcf5
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
 
