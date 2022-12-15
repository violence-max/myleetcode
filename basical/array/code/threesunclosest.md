题目描述：  
![image](/basical/array/image/image22.png)  
解决过程：  
我一直看不懂题目的意思，不知道如果遇到和target值相等的三个数怎么办，感觉题目有点憨批，也没认真做，看了题解知道意思就瞬间ac了  
代码：  
```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        int n = nums.size();
        int MAX = INT_MAX;
        int ans = INT_MAX;
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i-1]) continue;
            int left = i + 1,right = n - 1;
            while (left < right) {
                int add = nums[i] + nums[left] + nums[right];
                if (add == target) {
                    return add;
                }
                if (abs(add - target) < MAX) {
                    MAX = abs(add - target);
                    ans = add;
                }
                if (add > target) {
                    right -= 1;
                    while (left < right && nums[right] == nums[right+1]) {
                        right--;
                    }
                } else {
                    left++;
                    while (left < right && nums[left] == nums[left-1]) {
                        left++;
                    }
                }
            } 
        }
        return ans;
    }
};
```