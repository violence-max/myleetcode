题目描述：  
![image](/basical/array/image/image17.png)  
解题过程：  
* 和三数之和一样  
代码：  
```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        int length = nums.size();
        vector<vector<int>> result;
        for (int i = 0; i < length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int left = i + 1; left < length - 2; left++) {
                if (left > i + 1 && nums[left] == nums[left - 1]) continue;
                int right = length - 1;
                for (int left2 = left + 1; left2 < length - 1; left2++) {
                    if (left2 > left + 1 && nums[left2] == nums[left2 - 1]) continue;
                    long sum = (long)nums[i] + nums[left] + nums[left2] + nums[right];
                    while (left2 < right && sum > target) {
                        sum = (long)nums[i] + nums[left] + nums[left2] + nums[--right];
                    }
                    if (right == left2) break;
                    else if (sum == target) {
                        result.push_back({nums[i],nums[left],nums[left2],nums[right]});
                    }
                }
            }
        }
        return result;
    }
};
```