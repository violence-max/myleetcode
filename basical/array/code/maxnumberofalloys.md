题目描述：

![image](/basical/array/image/image110.png)

解决过程：

第363场周赛t3，ac

- 二分答案

代码：

```cpp
class Solution {
public:
    int maxNumberOfAlloys(int n, int k, int budget, vector<vector<int>>& composition, vector<int>& stock, vector<int>& cost) {
        int ans = 0;
        int c[k], idx = 0;
        for (auto& v : composition) {
            int sum = 0;
            for (int i = 0; i < n; ++i) {
                sum += v[i] * cost[i];
            }
            c[idx++] = sum;
        }
        for (int x = 0; x < k; ++x) {
            auto &v = composition[x];
            int mnn = 0, mxx = INT_MIN;
            for (int i = 0; i < n; ++i) {
                mxx = max(mxx, (stock[i] + v[i] - 1) / v[i]);
            }
            int left = mnn, right = mxx, spend = 0;
            while (left <= right) {
                int mid = (left + right) >> 1;
                long long buy = 0;
                for (int i = 0; i < n; ++i) {
                    buy += (long long)mid * v[i] <= stock[i] ? 0 : ((long long)mid * v[i] - stock[i]) * cost[i];
                    if (buy > budget) break;
                }
                if (buy <= budget) {
                    spend = buy;
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
            int cnt = max(0, left - 1);
            cnt += cnt == mxx ? (budget - spend) / c[x] : 0;
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```