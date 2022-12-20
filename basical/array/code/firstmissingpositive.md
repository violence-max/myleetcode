题目描述：  
![image](/basical/array/image/image27.png)  
解决过程：  
想了挺久的想不出来，然后看了题解，发现了置换的两种方法：一种是通过替换负数把对应位置的值标记出来，一种是通过直接交换来标示。  
这两种方法的理论基础其实是最小的没有出现的正整数只能出现在[1,n+1]的范围内。如果把对应的值换到对应的位置上就可以说明那个位置上已经出现了应该出现的值。  
负号占位法：  
1. 第一遍扫描把所有小于等于0的数变为n+1
2. 第二遍扫描把所有在n的范围内的数对应的位置都取负号，标志着该数已经出现
3. 第三遍扫描找出第一个大于0的数，如果不存在则表示答案是n+1  
置换方法：  
1. 第一遍扫描把所有大于0的并且在n的范围内的数换到自己的位置上
2. 第二遍扫描找出第一个不在位置上的数，如果不存在则答案是n+1  
代码：  
```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            if (nums[i] <= 0) {
                nums[i] = n + 1;
            }
        }
        for (int i = 0; i < n; i++) {
            int num = abs(nums[i]);
            if (num <= n) {
                nums[num-1] = -abs(nums[num-1]);
            }
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) {
                return i + 1;
            }
        }
        return n + 1;
    }
};
```  
```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                swap(nums[nums[i] - 1], nums[i]);
            }
        }
        for (int i = 0; i < n; ++i) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        return n + 1;
    }
};
```