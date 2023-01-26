题目描述：  
![image](/algorithmn/dynamic_programming/image/image46.png)  
解题过程：  
woshishabi  
写了两个小时没写出来，题解代码就不超过15行  
具体见代码吧，与最大子数组和唯一的区别就在于这里可以负负得正，因此需要记录最小乘积子数组  
代码：  
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        int maxf = nums[0], minf = nums[0], ans = nums[0];
        for (int i = 1; i < n; i++) {
            int mx = maxf, mn = minf;
            maxf = max(mx * nums[i], max(mn * nums[i],nums[i]));
            minf = min(mn * nums[i], min(mx * nums[i],nums[i]));
            ans = max(maxf,ans);
        }
        return ans;
    }
};
```