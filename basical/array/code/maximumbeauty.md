题目描述：

![image](/basical/array/image/image93.png)

解决过程：

第354场周赛t2，15分钟

代码：

```cpp
class Solution {
public:
    int maximumBeauty(vector<int>& nums, int k) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int ans = 1;
        int left = 0, right = 0;
        for (int i = 1; i < n; ++i) {
            while (nums[i] - k > nums[left] + k) {
                left++;
            }
            right++; 
            // cout << ans << ' ' << left << ' ' << right << endl;
            ans = max(ans, right - left + 1);
        }
        return ans;
    }
};
```