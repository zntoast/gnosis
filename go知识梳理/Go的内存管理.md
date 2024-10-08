go 的内存管理主要包括 内存分配和内存回收两个方面

1、内存分配
    - 堆内存分配 ： 大部分对象在堆上分配，由垃圾回收器管理，分配主要通过 mallocgc函数实现，该函数负责在堆上分配内存，并进行垃圾回收。主要步骤包括
        - 分配内存 ： 根据对象大小选择合适的内存块
        - 初始化内存 ： 将分配的内存初始化为零值
        - 垃圾回收标记 ： 将新分配的对象标记为可达
    - 栈内存分配 ： 小对象和临时对象在栈上分配，由编译器管理，性能更高。主要用于函数和局部变量，由编译器在编译阶段进行优化，栈内存分配的主要特点包括
        - 快速分配 ： 栈内存分配速度快，无需垃圾回收
        - 自动管理 ： 栈上的对象在函数返回时自动释放
    - 内存池 ： GO语言使用内存池 （mcache、mcetral、mheap）来管理内存碎片和分配开销
2、垃圾回收
    
- go语言的垃圾回收负责自动回收不再使用的内存，采用了并发标记清楚算法，减少程序的停顿时间。
- 垃圾回收的基本概念
    - 标记（mark）： 从跟对象开始，遍历所有可达对象，并标记它们
    - 清除（sweep）： 清除未被标记的对象，回收他们占用的内存
- 三色标记法
    - 白色 ： 未被访问的对象
    - 灰色 ： 已被访问但未完全扫描其引用的对象
    - 黑色 ： 已被访问且引用已被完全扫描的对象
- 写屏障（write barrier）在并发标记过程中，写屏障用户确保对象状态的一致性，当一个对象的引用被修改时，写屏障会触发，确保垃圾回收器能够正确处理对象的状态变化
    
- 当栈内存使用达到一定的阈值时，垃圾回收器会被触发，开发者也可以使用runtime.GC（）手动触发垃圾回收
    
- 垃圾回收的性能优化
	- 减少对象的分配 ：尽量减少不必要的对象分配，使用对象池 sync.Pool 来重用对象
	- 减少堆内存使用：尽量将对象分配在栈上，
	- 调整gc参数

### 总结：

```
Go语言的垃圾回收器通过标记清楚算法自动管理内存，减少了开发者的负担，提高了程序的性能和可靠性，理解垃圾回收的基本原理和优化方法，有助于编写高效、稳定的go程序
```

