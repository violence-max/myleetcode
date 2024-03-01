题目描述：

![image](/basical/array/image/image87.png)

解决过程：
第352场周赛t3，没做出来，没有想到这样的不间断子数组中最多只能有三个数据

代码：

```cpp
class Solution {
public:
    long long continuousSubarrays(vector<int>& nums) {
        long long ans = 0;
        int n = nums.size();
        multiset<int> p;
        int left = 0;
        for (int right = 0; right < n; ++right) {
            p.insert(nums[right]);
            while (!p.empty() && *p.rbegin() - *p.begin() > 2) {
                p.erase(p.find(nums[left++]));
            }
            ans += right - left + 1;
        }
        return ans;
    }
};
```