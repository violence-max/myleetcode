题目描述：

![image](/basical/array/image/image83.png)

解决过程：

第107场双周赛t4，没做出来

- 离线查询+滑动窗口

代码：

```cpp
class Solution {
public:
    vector<int> countServers(int n, vector<vector<int>>& logs, int x, vector<int>& queries) {
        sort(logs.begin(), logs.end(), [&](const vector<int>& u, const vector<int>& v){
            return u[1] < v[1];
        });
        int lq = queries.size(), ll = logs.size();
        vector<int> indices (lq);
        iota(indices.begin(), indices.end(), 0);
        sort(indices.begin(), indices.end(), [&](int a, int b){
            return queries[a] < queries[b];
        });
        unordered_map<int, int> cnt;
        queue<int> q;
        vector<int> ans (lq);
        int ptr = 0;
        // cout << "here" << endl;
        for (int i = 0; i < lq; ++i) {
            int left = queries[indices[i]] - x, right = queries[indices[i]];
            // cout << left << ' ' << right << endl;
            while (!q.empty() && logs[q.front()][1] < left) {
                if (cnt[logs[q.front()][0]] == logs[q.front()][1]) {
                    cnt.erase(logs[q.front()][0]);
                }
                q.pop();
            }
            // cout << "here" << endl;
            while (ptr < ll && logs[ptr][1]  <= right) {
                if (logs[ptr][1] >= left) {
                    q.push(ptr);
                    cnt[logs[ptr][0]] = logs[ptr][1];
                }
                ptr++;
            }
            // cout << ptr << endl;
            // for (auto& p : cnt) {
            //     cout << p.first << ' ' << p.second << endl;
            // }
            ans[indices[i]] = n - cnt.size();
        }
        return ans;
    }
};
```