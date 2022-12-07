题目描述：  
![image](/algorithmn/dynamic_programming/image/image26.png)  
解决过程：  
简单题，直接秒杀好吧  
代码：  
```cpp
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int n = nums.size(),len = 1,result = 1;
        int MAX = nums[0];
        for (int i = 1; i < n; i++) {
            if (nums[i] > MAX) {
                MAX = nums[i];
                len++;
                result = max(result,len);
            } else {
                len = 1;
                MAX = nums[i];
            }
        }
        return result;
    }
};
```