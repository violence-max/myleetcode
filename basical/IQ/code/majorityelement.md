题目描述：  
![image](/basical/IQ/image/image19.png)  
解决过程：  
哈希表肯定很简单，但是官方解答给出了五种解法：  
1. 暴力
2. 排序：排序后中点值一定为众数
3. 哈希
4. 递归：如果a是众数，则将数组分成两部分以后a至少时一部分的众数
5. 投票算法：票数多的候选人会当选  
复习时直接复习投票算法就好，题解讲得比较清楚了  
代码：（投票算法）  
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int candidate = -1, count = 0;
        for (int num : nums) {
            if (num == candidate) ++count;
            else if (--count < 0) {
                candidate = num;
                count = 1;
            }
        }
        return candidate;
    }
};
```