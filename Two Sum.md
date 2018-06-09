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
> leetcode上提交的代码
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> index;
        for(int i = 0; i < nums.size(); i++){
            for(int j = i+1; j < nums.size(); j++){
                if(nums.at(i) + nums.at(j) == target){
                    index.push_back(i);
                    index.push_back(j);
                }
            }
        }
        return index;
    }
};
```
结果：  
AC  
Your runtime beats 11.12 % of cpp submissions.  
>>注意，如果是在CLion上调试的话：  
main.cpp:
```
#include <vector>
#include "Solution.h"
using namespace std;

int main(){
    vector<int> input,output;
    input.push_back(2);
    input.push_back(3);
    input.push_back(4);
    input.push_back(5);
    input.push_back(6);
    input.push_back(5);

    Solution sol1;
    output = sol1.twoSum(input,8);

    for(int k = 0; k < output.size(); k++){
        cout << output[k] << endl;
    }

    return 0;
 }
```  
Solution.h:
```
#ifndef LIANSHOU_SOLUTION_H
#define LIANSHOU_SOLUTION_H

#include <vector>

class Solution {
public:
    std::vector<int> twoSum(std::vector<int>& data, int target);
};

#endif 
```  
Solution.cpp:  
```
#include "Solution.h"
#include <iostream>
#include <vector>
using namespace std;

vector<int> Solution::twoSum(vector<int>& data, int target){
    vector<int> index;
    for (int i = 0; i < data.size(); i++){
        for(int j = i + 1; j < data.size(); j++){
            if(data.at(i) + data.at(j) == target){
                index.push_back(i);
                index.push_back(j);
            }
        }
    }
    return index;
}
```
