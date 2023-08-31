题目描述：

![image](/algorithmn/greed/image/image27.png)

解决过程：

第357场周赛t4，没做出来

- 反悔贪心：排序+贪心（基于相对值的比较）

代码：

```
class Solution {
public:
    long long findMaximumElegance(vector<vector<int>>& items, int k) {
        sort(items.begin(), items.end(), [&](auto& a, auto& b){
            return a[0] > b[0];
        });
        long long ans = 0;
        int n = items.size();
        long long total_profit = 0;
        unordered_set<int> s;
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            if (i < k) { // 先贪心地认为前k个利润最大地商品最优雅
                total_profit += items[i][0];
                if (!s.count(items[i][1])) s.insert(items[i][1]);
                else stk.push(i);
            } else if (!stk.empty() && !s.count(items[i][1])) {
                // 后n-k个商品想要替换上来，其贡献必须足够，由于经过排序之后
                // profit不可能比之前的大，因此必须是不同的种类才有可能贡献才足够

                total_profit += items[i][0] - items[stk.top()][0];
                s.insert(items[i][1]);
                stk.pop();
            }
            ans = max(ans, (long long)(total_profit + (long long)s.size() * s.size()));
            // cout << i << ' ' << total_profit << ' ' << s.size() << ' ' << ans << endl;
        }
        return ans;
    }
};
```