# leetcode Two Sum
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.  
You may assume that each input would have exactly one solution, and you may not use the same element twice.

**Example:**
> Given nums = [2, 7, 11, 15], target = 9,  
Because nums[0] + nums[1] = 2 + 7 = 9,  
return [0, 1].

#### 解法1:暴力解法
两个嵌套的for循环。  
第一个for循环遍历数组的每一个元素，对于当前的元素，第二个for循环则遍历数组中的所有其他元素，寻找是否有与之满足条件的数。  
这个算法的时间复杂度为O(n^2)，空间复杂度为O(1)。
