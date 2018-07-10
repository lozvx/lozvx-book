# B-Tree

[https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/03.02.md](https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/03.02.md)



动态查找树主要有：二叉查找树（Binary Search Tree），平衡二叉查找树（Balanced Binary Search Tree），[红黑树](http://blog.csdn.net/v_JULY_v/article/category/774945)\(Red-Black Tree \)，B-tree/B+-tree/ B\*-tree \(B~Tree\)。前三者是典型的二叉查找树结构，其查找的时间复杂度`O(log2N)`与树的深度相关，那么降低树的深度自然会提高查找效率。  


二叉查找树结构由于**树的深度过大而造成磁盘I/O读写过于频繁，进而导致查询效率低下**，因此我们该想办法降低树的深度，从而减少磁盘查找存取的次数。这里引出了平衡多路查找树，即**B-tree（B-tree树即B树）**



B树与红黑树最大的不同在于，B树的结点可以有许多子女，从几个到几千个。不过B树与红黑树一样，一棵含n个结点的B树的高度也为`O(lgn)`，但可能比一棵红黑树的高度小许多，因为它的分支因子比较大。所以，B树可以在`O（logn）`时间内，实现各种如插入（insert），删除（delete）等动态集合操作。  


[https://blog.csdn.net/qq\_26768741/article/details/53164202](https://blog.csdn.net/qq_26768741/article/details/53164202)

