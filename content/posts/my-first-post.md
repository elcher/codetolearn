---
title: "Day 1 数组"
date: 2022-10-26T13:56:51+08:00
draft: false
series: ["代码随想录算法训练营"]
categories: ["Algorithms", "Data Structures", "LeetCode"]
tags: ["array", "java", "python", "binarySearch", "twoPointers"]
comment: false
---
## [数组理论基础](https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80)
> 首先要知道数组在内存中的存储方式，这样才能真正理解数组相关的面试题


***
## Coding Problems
### [704.二分查找](https://leetcode.cn/problems/binary-search/) 
#### 看讲解前
这道题之前做过。这次做思路更清晰了，但是还是改了两次才通过。最后的通过代码如下。
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:    # left 可以等于 right
            pivot = left + (right - left) // 2
            if nums[pivot] == target:
                return pivot
            elif nums[pivot] < target:
                left = pivot + 1
            else:
                right = pivot - 1
                
        return -1               # 不要忘写

```

第一个错误是`while`循环结束后没写`return`语句，也就是没写第 13 行代码，结果找不到`target`时，输出了`None`而不是`-1`。这是个说大不大说小不小的错误。说不大是因为看到 output 和 expected 的对比，很容易就可以改对。说不小是因为在写代码的时候，一定要搞清楚对 output 的要求，不然会耽误很多时间 debug。

第二个错误是`while`语句的边界条件没考虑到`left == right`的情况，也就是第 4 行代码的条件里只写了小于没写等于。这属于经验主义错误，写代码时没认真考虑。当数组只有一个数值时，`left == right`，这没有任何问题，所以之前是做什么题的时候让自己变得如此畏畏缩缩不敢加那个小小的`=`呢？感觉可以以后留心总结一下。

下面贴一下我上次做这题时写的答案吧。感觉自己进步了。过去写的代码好繁琐啊，为啥一定要追求指针也是答案这个点呢？现在的我可能审美变了，不是很能理解当时的想法了。
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if not nums:
            return -1
        
        left, right = 0, len(nums) - 1
        while left + 1 < right:
            pivot = (left + right) // 2 # 有数值溢出的风险
            
            if nums[pivot] < target:
                left = pivot
            elif nums[pivot] == target:
                right = pivot 
            else:
                right = pivot 
        
        if nums[left] == target:
            return left
        if nums[right] == target:
            return right
                
        return -1
```

#### 看讲解后
待更新
### [27.移除元素](https://leetcode.cn/problems/remove-element/)
#### 看讲解前
这是一道没做过的题。但是感觉关键在于找到`val`和去掉`val`这两步。找到`val`很容易，上一题已经实践过了。去掉`val`是这题需要解决的新问题。Carl 讲过的数组的底层结构对解决这个问题就很关键了。数组里删去元素更像是用目标坐标后的元素依次往前挪一位，所以我想到的办法就是用一个`while`循环。代码如下。
```python {hl_lines=[10, 11, 12, 13]}
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        nums.sort()
        left, right = 0, len(nums) - 1
        count = 0
        while left <= right:
            pivot = left + (right - left) // 2
            if nums[pivot] == val:
                count += 1
                while pivot + 1 < len(nums):
                    nums[pivot] = nums[pivot + 1]
                    pivot += 1
                right -= 1
            elif nums[pivot] < val:
                left = pivot + 1
            else:
                right = pivot - 1
        return len(nums) - count
```
#### 看讲解后
待更新
