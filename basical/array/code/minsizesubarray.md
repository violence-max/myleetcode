题目描述：

![image](/basical/array/image/image114.png)

解决过程：

第365场周赛t3，没做出来

- 滑动窗口

代码：

```cpp
class Solution {
public:
    int minSizeSubarray(vector<int>& nums, int target) {
        int n = nums.size();
        long long sum = 0;
        for (int i = 0; i < n; ++i) sum += nums[i];
        int cnt = target / sum;
        if (target % sum == 0) return cnt * n;
        target -= sum * cnt;
        for (int i = 0; i < n; ++i) {
            nums.push_back(nums[i]);
        }
        long long s = 0;
        int left = 0, l = INT_MAX;
        for (int right = 0; right < 2 * n; ++right) {
            s += nums[right];
            while (s >= target) {
                // cout << left << ' ' << right << ' ' << s << ' ' << target << endl;
                if (s == target) {
                    l = min(l, right - left + 1);
                }
                s -= nums[left++];
            }
        }
        return l == INT_MAX ? -1 : l + cnt * n;
    }
};
```