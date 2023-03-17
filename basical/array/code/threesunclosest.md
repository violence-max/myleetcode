题目描述：  
![image](/basical/array/image/image22.png)  
解决过程：  
* 与三数之和的区别在于当我们遇到三数之和等于target的时候可以直接返回，否则我们需要设定一个差值来不断寻找这个最小的差值
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