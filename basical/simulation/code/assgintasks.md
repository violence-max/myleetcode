题目描述：

![image](/basical/simulation/image/image5.png)

解题过程：

写了十几分钟，debug了半个多小时，最后还是看了题解，发现更新时间戳的时候需要把新的时间戳所有已经完成的任务都给pop出来，但是我觉得只pop一个也没问题啊。。。百思不得其解

代码：

```cpp
class Solution {
public:
    vector<int> assignTasks(vector<int>& servers, vector<int>& tasks) {
        int n = servers.size(), m = tasks.size();
        auto cmp = [&](int i, int j) -> bool {
            return servers[i] > servers[j] || (servers[i] == servers[j] && i > j);
        };
        priority_queue<int, vector<int>, decltype(cmp)> s(cmp);
        vector<int> id (n);
        iota(id.begin(), id.end(), 0);
        for (auto i : id) {
            // cout << i << endl;
            s.push(i);
        }
        // cout << "88" << endl;
        vector<int> cost (n);
        auto cmp2 = [&](int i, int j) -> bool {
            return cost[i] > cost[j] || (cost[i] == cost[j] && i > j);
        };
        priority_queue<int, vector<int>, decltype(cmp2)> e(cmp2);
        vector<int> ans (m);
        // cout << "here" << endl;
        int t = 0;
        auto release = [&]() {
            while (!e.empty() && cost[e.top()] <= t) {
                int idx = e.top();
                // cout << idx << endl;
                e.pop();
                s.push(idx);
            }
        };
        for (int i = 0; i < m; ++i) {
            t = max(t, i);
            release();
            // cout << s.size() << endl;
            if (s.empty()) {
                t = cost[e.top()];
                release();
            }
            int nxt = s.top();
            s.pop();
            ans[i] = nxt;
            cost[nxt] = t + tasks[i];
            // cout << i << ' ' << nxt << ' ' << cost[nxt] << endl;
            e.push(nxt);
        }
        return ans;
    }
};
```