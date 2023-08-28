题目描述：

![image](/basical/array/image/image98.png)

解决过程：

第359场周赛t4，ac

- 滑动窗口

代码：

```cpp
class Solution {
public:
    int pos[100020], idx[100020];
    bool vis[100020];
    int longestEqualSubarray(vector<int>& nums, int k) {
        int n = nums.size();
        memset(pos, -1, sizeof pos);
        memset(idx, -1, sizeof idx);
        memset(vis, 0, sizeof vis);
        for (int i = n - 1; i >= 0; --i) {
            if (pos[nums[i]] != -1) {
                idx[i] = pos[nums[i]];
            }
            pos[nums[i]] = i;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (!vis[nums[i]]) {
                vis[nums[i]] = true;
                
                int cnt = 1, cost = 0, ret = 1;
                int left = i, right = i;
                while (idx[right] != -1) {
                    cost += idx[right] - right - 1;
                    ++cnt;
                    while (cost > k) {
                        cost -= idx[left] - left - 1;
                        left = idx[left];
                        --cnt;  
                    }
                    ret = max(ret, cnt);
                    right = idx[right];
                }
                ans = max(ans, ret);
            }
        }
        return ans;
    }
};
```