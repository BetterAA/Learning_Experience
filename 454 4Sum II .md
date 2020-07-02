tags：[ leetcode,medium ]
	
	- leetcode
	- medium
	- 不花哨
---

## 454 4Sum II 
### 题目链接
> [题目链接](https://leetcode-cn.com/problems/4sum-ii/)

#### 描述
> Given four lists A, B, C, D of integer values, compute how many tuples (i, j, k, l) there are such that A[i] + B[j] + C[k] + D[l] is zero.

>To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228 to 228 - 1 and the result is guaranteed to be at most 231 - 1.

#### Test Case

> Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

>Output:
2

>Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0

#### 补充题解
#### My Code
```
	class Solution(object):
    def fourSumCount(self, A, B, C, D):
        """
        :type A: List[int]
        :type B: List[int]
        :type C: List[int]
        :type D: List[int]
        :rtype: int
        """
        '''
       (其实没有想的那么复杂。。。我一开始只想到1个排序后二分lg+3个遍历。。 但是因为这里不需要具体值，只要个数，所以用hashmap存个数就可以了
       下面还有我的一个错误想法
       )
       一.采用分为两组，HashMap存一组，另一组和HashMap进行比对。
        二.这样的话情况就可以分为三种：
        1.HashMap存一个数组，如A。然后计算三个数组之和，如BCD。时间复杂度为：O(n)+O(n^3),得到O(n^3).
        2.HashMap存三个数组之和，如ABC。然后计算一个数组，如D。时间复杂度为：O(n^3)+O(n),得到O(n^3).
        3.HashMap存两个数组之和，如AB。然后计算两个数组之和，如CD。时间复杂度为：O(n^2)+O(n^2),得到O(n^2).
        三.根据第二点我们可以得出要存两个数组算两个数组,相当于是2sum的扩展板。
        四.我们以存AB两数组之和为例。首先求出A和B任意两数之和sumAB，以sumAB为key，sumAB出现的次数为value，存入hashmap中。
        然后计算C和D中任意两数之和的相反数sumCD，在hashmap中查找是否存在key为sumCD。
        算法时间复杂度为O(n2)。
        ''' 
        sumx = {}
        for a in A:
            for b in B:
                cur_sum = a+b
                sumx[cur_sum] = sumx.setdefault(cur_sum,0) + 1
        
        # 这里一开始我的错误想法，AB和CD分别算一个map， 最后遍历两个map判断和，明显有问题，遍历map的规模更大。。 而直接是遍历CD时候判断就行了
        res = 0
        for c in C:
            for d in D:
                need = -(c+d)
                res += sumx.get(need,0)

        return res 
```