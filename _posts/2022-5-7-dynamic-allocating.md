## malloc()是怎么工作的？
- 为什么需要malloc？ 作用是什么？
- 对虚拟内存的哪个区域产生影响？
- 有哪几种申请内存的方式？
  - mmap
    - 具体是怎么实现的? (COW策略,COW策略是针对PM的,VM中是直接在映射区域进行修改)
  - brk
  - 以上两种有什么区别？分别用在什么场合？
- malloc(1)时,分配了多大的内存空间? free时会归还给操作系统嘛?
  
  

## free()怎么工作的?
- 怎么追踪空闲块?
  - 有哪几种方式,分别怎么实现? 