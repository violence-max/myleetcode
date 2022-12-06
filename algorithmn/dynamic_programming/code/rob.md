题目描述：  
![image](/algorithmn/dynamic_programming/image/image18.png)  
解题过程：  
没有想到这道题我居然想不出来，其实我已经有动态规划方程的意识了，但是没有想出具体一点的公式以及一些初始情况。  
题解还是思路太过于简单了，只是考虑了如果要盗窃当前的屋子则不能盗窃上一间屋子，要么就不盗窃这一间屋子只盗窃上一间屋子。  
强无敌！  
代码：  
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) return 0;
        else if (n == 1) return nums[0];
        else {
            int first = nums[0],second = max(nums[0],nums[1]);
            for (int i = 2; i < n; i++) {
                int temp = second;
                second = max(first+nums[i],second);
                first = temp;
            }
            return second;
        }
    }
};
```