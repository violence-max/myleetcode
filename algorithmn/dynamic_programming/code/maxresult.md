题目描述：

![image](/algorithmn/dynamic_programming/image/image101.png)

解决过程：
ac，用的是multiset，很慢，零神有更好的解法

代码：（multiset→优先队列→单调队列）

```cpp
class Solution {
public:
    int dp[100020];
    int maxResult(vector<int>& nums, int k) {
        memset(dp, 0, sizeof dp);
        int n = nums.size();
        multiset<int> s;
        for (int i = 0; i < n; ++i) {
            dp[i] = nums[i] + (s.empty() ? 0 : *s.rbegin());
            if (i >= k) {
                s.erase(s.find(dp[i-k]));
            }
            s.insert(dp[i]);
        }
        return dp[n-1];
    }
};
```

```cpp
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        
        // 优先队列中的二元组即为 (f[j], j)
        priority_queue<pair<int, int>> q;
        q.emplace(nums[0], 0);
        int ans = nums[0];
        
        for (int i = 1; i < n; ++i) {
            // 堆顶的 j 不满足限制
            while (i - q.top().second > k) {
                q.pop();
            }
            
            ans = q.top().first + nums[i];
            q.emplace(ans, i);
        }
        
        return ans;
    }
};
```

```cpp
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int n = nums.size();
        
        // 单调队列中的二元组即为 (f[j], j)
        deque<pair<int, int>> q;
        q.emplace_back(nums[0], 0);
        int ans = nums[0];
        
        for (int i = 1; i < n; ++i) {
            // 队首的 j 不满足限制
            while (i - q.front().second > k) {
                q.pop_front();
            }
            
            ans = q.front().first + nums[i];
            
            // 队尾的 j 不满足单调性
            while (!q.empty() && ans >= q.back().first) {
                q.pop_back();
            }
            
            q.emplace_back(ans, i);
        }
        
        return ans;
    }
};
```