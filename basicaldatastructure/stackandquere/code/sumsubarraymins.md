题目描述：

![image](/basicaldatastructure/stackandquere/image/image28.png)

解决过程：

好像花了半个小时。。主要是重复元素的问题

代码：（单调栈→动态规划（dp[i]表示所有以arr[i]结尾的子数组的最小值之和））

```cpp
class Solution {
public:
    int mod = 1e9 + 7;
    int left[30020], right[30020];
    int sumSubarrayMins(vector<int>& arr) {
        int n = arr.size();
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && arr[stk.top()] > arr[i]) {
                stk.pop();
            }
            left[i] = stk.empty() ? -1 : stk.top();
            stk.push(i);
            // cout << i << ' ' << left[i] << endl;
        }
        stack<int> s;
        for (int i = n - 1; i >= 0; --i) {
            while (!s.empty() && arr[s.top()] >= arr[i]) {
                s.pop();
            }
            right[i] = s.empty() ? n : s.top();
            s.push(i);
            // cout << i << ' ' << right[i] << endl;
        }
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            int cnt = (i - left[i]) * (right[i] - i);
            // cout << i << ' ' << cnt << endl;
            ans = (ans + (long long)cnt * arr[i]) % mod;
        }
        return ans;
    }
};
```

```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {
        int n = arr.size();
        long long ans = 0;
        long long mod = 1e9 + 7;
        stack<int> monoStack;
        vector<int> dp(n);
        for (int i = 0; i < n; i++) {
            while (!monoStack.empty() && arr[monoStack.top()] > arr[i]) {
                monoStack.pop();
            }
            int k = monoStack.empty() ? (i + 1) : (i - monoStack.top());
            dp[i] = k * arr[i] + (monoStack.empty() ? 0 : dp[i - k]);
            ans = (ans + dp[i]) % mod;
            monoStack.emplace(i);
        }
        return ans;
    }
};
```