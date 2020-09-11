## 字典分词

### 字典

#### 前缀树

利用词的共同前缀以达到节省空间的目的。例如，四个单词bachelor, baby, badge, jar的Trie树如下：

![img](https://images2015.cnblogs.com/blog/399159/201701/399159-20170109144930275-1379994294.png)

#### 双数组字典树

为了改进Trie的效率，Double-array结合了array查询效率高、list节省空间的优点，具体是通过两个数组`base`、`check`来实现。

实例 https://zhuanlan.zhihu.com/p/35193582 

代码 https://linux.thai.net/~thep/datrie/datrie.html  

### 查字典

给定一个目标串。对其分词，主要方法有正相匹配、反相匹配、最长匹配、最短匹配，以及这几种方式的组合。

### 串匹配

#### 单模匹配-KMP

[http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html](http://www.ruanyifeng.com/blog/2013/05/Knuth–Morris–Pratt_algorithm.html)

KMP算法用于单模匹配，比如在一个目标串当中匹配一个模式串。暴力解法就是扫描目标串与模式串，如果发现不匹配，则目标串起始点回溯到原起始点，再后移一位，而模式串回溯到开头0。
KMP算法能够高效的找出目标串中的匹配串，相比于暴力搜索的O(m*n)的时间复杂度，KMP算法的搜索复杂度为O(m+n)。

![img](http://www.ruanyifeng.com/blogimg/asset/201305/bg2013050107.png)

![img](http://www.ruanyifeng.com/blogimg/asset/201305/bg2013050111.png)

![img](http://www.ruanyifeng.com/blogimg/asset/201305/bg2013050112.png)

![img](http://www.ruanyifeng.com/blogimg/asset/201305/bg2013050113.png)

"部分匹配值"就是"前缀"和"后缀"的最长的共有元素的长度。以"ABCDABD"为例，

> ![img](http://www.ruanyifeng.com/blogimg/asset/201305/bg2013050109.png)





#### 多模匹配-AC自动机

kmp单模式匹配只需有一个模式（i.e. 词）；ac自动机为词典中每个词都建立一个部分匹配表。

部分匹配：前缀和后缀公共串的长度，例如，abcdab的部分匹配为2。



## 树结构

### 二叉查找树bst



### 平衡二叉树avl

左子树<父节点大<右子树，采用二分法思维把数据按规则组装成一个树形结构的数据，用这个树形结构的数据减少无关数据的检索，大大的提升了数据检索的速度；

### 红黑树

一种弱平衡二叉树，减少旋转操作（实现更快的增删），但查找稍慢于avl。适用于频繁增删操作场景，如java treeMap

### B/B+ 树

B树（平衡多路查找树，为减少磁盘io，通过多路降低树高度的数据结构）
所有叶子节点深度相同，每个节点存储的数值更多，因此深度小于平衡二叉树

B+树（相对于B树，只有叶子节点含有value，中间节点不含）
由于非叶子节点中不承载数据，因此非叶子节点的每一层可以容纳更多的元素，降低了树的高度，减少了磁盘I/O的次数
所有元素都在叶子节点，查找性能稳定

B/B+ 树 用于 FS mysql的索引，原因是tree 深度低，有序。便于查找时尽可能减少IO。