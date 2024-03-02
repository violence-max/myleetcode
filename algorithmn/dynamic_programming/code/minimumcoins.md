题目描述：

![image](/algorithmn/dynamic_programming/image/image107.png)

解决过程：

第118场双周赛t3，没做出来

代码：（dfs→动规→单调队列）

```cpp
class Solution {
public:
    int memo[1020];
    int minimumCoins(vector<int>& prices) {
        int n = prices.size();
        memset(memo, -1, sizeof memo);
        function<int(int)> dfs = [&](int i) -> int {
            if (2 * i >= n) {
                return prices[i-1];
            }
            int &res = memo[i];
            if (res != -1) return res;
            res = INT_MAX;
            for (int j = i + 1; j <= 2 * i + 1; ++j) {
                res = min(res, dfs(j));
            }
            res += prices[i-1];
            return res;
        };
        return dfs(1);
    }
};
```

```cpp
class Solution {
public:
    int minimumCoins(vector<int> &prices) {
        int n = prices.size();
        for (int i = (n + 1) / 2 - 1; i > 0; i--) {
            prices[i - 1] += *min_element(prices.begin() + i, prices.begin() + i * 2 + 1);
        }
        return prices[0];
    }
};
```

```cpp
class Solution {
public:
    int minimumCoins(vector<int> &prices) {
        int n = prices.size();
        deque<pair<int, int>> q;
        q.emplace_front(n + 1, 0); // 哨兵
        for (int i = n; i ; i--) {
            while (q.back().first > i * 2 + 1) { // 右边离开窗口
                q.pop_back();
            }
            int f = prices[i - 1] + q.back().second;
            while (f <= q.front().second) {
                q.pop_front();
            }
            q.emplace_front(i, f); // 左边进入窗口
        }
        return q.front().second;
    }
};
```