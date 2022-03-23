### 1:数组  Array 命名空间：using System

    优点:内存：连续存储 因此索引速度快   赋值和修改元素简单 时间复杂度为O(1)
    缺点:因为是分配在连续内存所以要提前确定内存大小 空间的连续导致了存储效率低 插入和删除元素效率低
    数组中的浅拷贝问题：
    int[] newArray = array;
    把数组array赋值给newArray,这是一个浅拷贝的问题，就是把栈中指向堆中的地址赋值给新数组，C#中的数组元素存在堆中，newArray和array指向的是同一个数组，如果改变了array中的值，newArray中的值也会改变。如果想实现真正的拷贝，深拷贝用CopyTo,   array.CopyTo(newArray,0);就是把array中的值拷贝给newArray，实现深拷贝。
### 2:ArrayList  命名空间System.Collections
    优点：大小是按照其中存储的数据来动态扩充与收缩的。所以声明ArrayList对象时并不需要指定它的长度。ArrayList继承了IList接口，所以它可以很方便的进行数据的添加，插入和移除  可以存储不同数据类型的数据
    缺点：因为可以插入不同数据类型（把所有的数据都当做了Object类型来处理）会存在装箱拆箱操作装箱与拆箱的过程是很损耗性能的。
