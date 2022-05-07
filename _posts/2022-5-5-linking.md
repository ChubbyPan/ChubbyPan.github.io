## **WHY**
**为什么出现了linking？**

如果没有linking，程序内所有的代码都写在一个大文件当中，如果有一些函数经常出现，也不能复用，需要多次重复编写；如果有少量代码需要修改，也不能仅对产生修改的代码进行编译，需要浪费时间去编译那个大文件！


## **WHAT**
**1. 什么是linking？**
> (CSAPP P670) Linking is the process of collecting and combining various pieces of code and data into a single file that can be loaded(copied) into memory and executed.

将代码文件分开编译之后（形成.o文件），relocated到一个文件当中，这个文件就是exe可执行文件。这个可执行文件可以被放到内存当中并执行，

**2. linking发生在什么时候？**
   - compile time（静态链接）
   - load time（动态链接）
   - run time（动态链接）

从.o文件链接和.so文件链接两个角度说

## WHAT and WHY
**1. 静态链接**

函数和数据都被编译到一个二进制文件当中（ELF文件 可执行）

使用静态库时，linker把库中的函数和数据编译，和程序的其他模块组合起来（编译好的库文件复制进来），放入到可执行文件中。

  - pros： 因为已经事先编译好，都在同一个可执行文件当中，执行的时候速度会比较快
  - cons： （从空间上说）每个可执行程序当中有所有目标文件的副本，如果某一个目标文件需要复用在其他程序里，这个目标文件就会存在多个副本；（从更新上说）如果库文件发生更改，需要重新编译->链接->加载->执行

![static_linking](pics/static_linking.jpg)


**2. 动态链接**

在程序运行时，才把比如共享库里的文件链接起来。

区别在于，静态库 链接完再执行；动态库，一边链接一边执行；

-   pros：可以复用（同一段代码可以被多个runnning process使用）/更新方便，不需要将所有程序重新链接，只需要替换更新过的文件
-   cons：慢

![dynamic_linking](pics/dynamic_linking.jpg)


## **HOW**
**静态linking怎么实现？**
  1. symbol resolution 符号解析
  2. relocation
   
  relocation过程：

   ![how to linking](pics/how_to_linking.jpg)

  loading过程：

  ![loading in memory](pics/loading_in_memory.jpg)


### **appendix**
ELF(可执行标准文件格式及作用)

![ELF](pics/ELF.jpg)

.bss section存放未初始化的static 变量以及被初始化为0的全局变量和static 变量