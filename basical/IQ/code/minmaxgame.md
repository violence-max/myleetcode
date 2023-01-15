题目描述：  
![image](/basical/IQ/image/image15.png)  
解题过程：  
一分钟秒杀  
代码：  
```cpp
class Solution {
public:
    int minMaxGame(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return nums[0];
        vector<int> newnums (n / 2);
        for (int i = 0; i < newnums.size(); i++) {
            if (i & 1) {
                newnums[i] = max(nums[2 * i],nums[2 * i + 1]);
            } else {
                newnums[i] = min(nums[2 * i],nums[2 * i + 1]);
            }
        }
        return minMaxGame(newnums);
    }
};
```