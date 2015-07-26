### Problems 1 : Average-case lower bounds on comparison sorting
***
In this problem, we prove an Ω(n lg n) lower bound on the expected running time of any deterministic or randomized comparison sort on n distinct input elements. We begin by examining a deterministic comparison sort A with decision tree TA. We assume that every permutation of A's inputs is equally likely.
			   			   		
**a.** Suppose that each leaf of TA is labeled with the probability that it is reached given a random input. Prove that exactly n! leaves are labeled 1/n! and that the rest are labeled 0.

**b.** Let D(T) denote the external path length of a decision tree T ; that is, D(T) is the sum of the depths of all the leaves of T. Let T be a decision tree with k > 1 leaves, and let LT and RT be the left and right subtrees of T. Show that D(T) = D(LT) + D(RT) + k.


d\(k\) = \\min_{1 \\le i \\le k-1}\\{d\(i\) + d\(k-i\) + k\\} )





### `Answer`
**a.**

总共有n!个排列，也就是n!种可能，所以概率当然是1/n!.

**b.**

因为T比RT和LT高度少1，所以有多少叶子节点，就需要多加这么多个1.很好思考～

**c.**

由前一小题可知，有k个节点的树有关系式D(T)=D(LT)+D(RT)+k.

那么很自然可以知道，就是左子树的叶子节点从1到k-1这么多可能.

![](http://latex.codecogs.com/gif.latex?
d\(k\) = D\(T\) = D\(LT\) + D\(RT\) + k = \\min_{1 \\le i \\le k-1}\\{d\(i\) + d\(k-i\) + k\\} )

**d.**

比较简单的求导.

![](http://latex.codecogs.com/gif.latex?
f\(i\) = i\\lg{i} + \(k-i\)\\lg\(k-i\) \\\\ ~ \\hspace{6 mm}
     f'\(i\) = \\lg{i} + 1 - \\lg\(k-i\) - 1 = \\lg\\frac{i}{k-i} \\\\ ~ \\hspace{6 mm}
     f'\(i\) = 0  \\Leftrightarrow \\lg\\frac{i}{k-i} = 0 \\Rightarrow i/\(k-i\) = 1 \\Rightarrow i = \\frac{k}{2} )

**e.**

TA一共有n!个叶子节点，所以有D(T) > d(n!) = Ω(n!lg(n!))

我们已经求出了外路径长度，总共有n!个概率相同的节点,所以最终有

![](http://latex.codecogs.com/gif.latex?
\\frac{\\Omega\(n!\\lg\(n!\)\)}{n!} = \\Omega\(\\lg\(n!\)\) = \\Omega\(n\\lg{n}\) )

**f.**
非随机化版本有n!种路径，囊括了所有可能. 而任何一种随机化都肯定在这n!里面，而且少了random的操作.


### Problems 2 : Sorting in place in linear time
***
Suppose that we have an array of n data records to sort and that the key of each record has the value 0 or 1. An algorithm for sorting such a set of records might possess some subset of the following three desirable characteristics:






**d.**
Can any of your sorting algorithms from parts (a)-(c) be used to sort n records with b- bit keys using radix sort in O(bn) time? Explain how or why not.

**e.**

Suppose that the n records have keys in the range from 1 to k. Show how to modify counting sort so that the records can be sorted in place in O(n + k) time. You may use O(k) storage outside the input array. Is your algorithm stable? (Hint: How would you do it for k = 3?)




两个指针，一个指向数组头，一个指向数组尾，同时向中间移动.

**c.**

merge-sort就可以

[stable in-place sort](http://www.codeproject.com/Articles/26048/Fastest-In-Place-Stable-Sort)

**d.**

要用基数排序. 必须使用stable的算法.
   
**e.**

[implementation](./exercise_code/in_place_counting_sort.py)

我的实现不是stable的


### Problems 3 : Sorting variable-length items
***
a. You are given an array of integers, where different integers may have different numbers of digits, but the total number of digits over all the integers in the array is n. Show how to sort the array in O(n) time.


### `Answer`

**a.**

用RADIX-SORT,不过要稍微变形下. 当遍历到某一个数字超过其位数时，用0替代，并且将该数字放在正常有数字的后面，以后就不用迭代了. 保证了O(n)的时间.

**b.**

reverse所有的字符串，用RADIX-SORT.当当遍历到某一个字符串超过其大小时,用空格代替(空格 > 其他字母),并且将该数字放在该组正常有数字的前面，以后不要迭代.排序后reverse回来. 保证了O(n)的时间.
 
假设我们要排序字符串d,c,b,bcd,bc,bd,bdc.过程如下:

0 | 1 | 2 | 3 | 4 | result | reverse
:---- :|:----:|:----:|:----:|:----:|:----:|:----:
d | **b** | b | | | b | b
c | dc**b** | d**c**b | cb | | cb | bc
b | c**b** | **c**b | **d**cb | dcb | dcb | bcd
dcb | d**b** | **d**b | db | | db | bd
cb | cd**b** | c**d**b | **c**db | cdb |cdb | bdc
db | **c** | c | | | c | c
cdb | **d** | d | | | d | d

### Problems 4 : Water jugs
***
Suppose that you are given n red and n blue water jugs, all of different shapes and sizes. All red jugs hold different amounts of water, as do the blue ones. Moreover, for every red jug, there is a blue jug that holds the same amount of water, and vice versa.




### `Answer`
**a.**

二重循环，比较所有的.

**b.**

决策树的每个node都有3个sub-node(A < B | A = B | A > B).总共有n!个输出.所以有

![](http://latex.codecogs.com/gif.latex?3^h \\ge n! \\Rightarrow h \\ge \\lg{n!} \\Rightarrow h = \\Omega\(n\\lg{n}\))

**c.**

key idea : 跟快速排序是一样的

[implementation](./exercise_code/water-jugs.py)


### Problems 5 : Average sorting
***
Suppose that, instead of sorting an array, we just require that the elements increase on average. More precisely, we call an n-element array A k-sorted if, for all i = 1, 2, . . ., n - k, the following holds:

![](http://latex.codecogs.com/gif.latex? \\frac{\\sum_{j = i}^{i+k-1}}{k}A[j] \\le \\frac{\\sum_{j = i+1}^{i+k}}{k}A[j])

**a.** What does it mean for an array to be 1-sorted?

**b.** Give a permutation of the numbers 1, 2, . . ., 10 that is 2-sorted, but not sorted.





### `Answer`
**a.**

该数组完全排好序.

**b.**

[2,1,3,4,5,6,7,8,9,10]
 
**c.**

这个证明很简单，移项就能得到.

**d.**

把数组分为k组，用heapsort/mergesort.

T = k * O((n/k)log(n/k)) = O(nlg(n/k)) 

**e.**

确实跟[练习6.5.8](https://github.com/gzc/CLRS/blob/master/C06-Heapsort/6.5.md#exercises-65-8)是一样的

**f.**

我们有d的结果，因为k是常量，所以可以忽略.

### Problems 6 : Lower bound on merging sorted lists
***

The problem of merging two sorted lists arises frequently. It is used as a subroutine of MERGE-SORT, and the procedure to merge two sorted lists is given as MERGE in Section 2.3.1. In this problem, we will show that there is a lower bound of 2n - 1 on the worst-case number of comparisons required to merge two sorted lists, each containing n items.





这个题目在[leetcode](https://leetcode.com/problems/merge-two-sorted-lists/)出现过



2^h \\ge \\frac{2n!}{\(n!\)^2}  \\\\ ~ \\hspace{8 mm}
h \\ge \\log{2n!} -2\\log{n!} \\\\ ~ \\hspace{10 mm}
= \\Theta\(2n\\log{2n}\) - 2\\Theta\(n\\log{n}\) \\\\~ \\hspace{10 mm}
= \\log{2}\\Theta\(2n\) \\\\~ \\hspace{10 mm}
= \\Theta\(2n\))

树的高度是2n级别的，因此最少需要2n-o(n)次




 
***
Follow [@louis1992](https://github.com/gzc) on github to help finish this task.
