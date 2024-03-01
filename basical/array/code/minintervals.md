题目描述：

![image](/basical/array/image/image93.png)

解决过程：

写了挺久的，但是思路挺快的，就是实现一直摇摆不定

代码：

```cpp
class Solution {
public:
    vector<int> minInterval(vector<vector<int>>& intervals, vector<int>& queries) {
        int n = intervals.size(), m = queries.size();
        vector<int> a(n), b(n), c(m), ans(m);       
        iota(a.begin(), a.end(), 0);
        iota(c.begin(), c.end(), 0);
        sort(a.begin(), a.end(), [&](int i, int j){
            return intervals[i][0] < intervals[j][0] || (intervals[i][0] == intervals[j][0] && intervals[i][1] < intervals[j][1]);
        });
        sort(c.begin(), c.end(), [&](int i, int j){
            return queries[i] < queries[j];
        });
        for (int i = 0; i < n; ++i) {
            b[i] = intervals[i][1] - intervals[i][0] + 1;
        }
        auto cmp = [&](int i, int j) -> bool {
            return b[i] > b[j];
        };
        priority_queue<int, vector<int>, decltype(cmp)> q(cmp);
        int ptr = 0;
        for (int x : c) {
            while (ptr < n && intervals[a[ptr]][0] <= queries[x]) {
                // cout << x << ' ' << queries[x] << ' ' << a[ptr] <<  endl;
                q.push(a[ptr++]);
            }
            while (!q.empty() && intervals[q.top()][1] < queries[x]) {
                q.pop();
            }
            if (!q.empty()) {
                ans[x] = b[q.top()];
            } else {
                ans[x] = -1;
            }
        }
        return ans;
    }
};
```