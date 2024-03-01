题目描述：

![image](/basicaldatastructure/stackandquere/image/image29.png)

解题过程：

做了好像挺久，wa四发，还是考虑不全面，看了题解发现set加lower_bound可以解决

代码：

```cpp
class Solution {
public:
    vector<int> avoidFlood(vector<int>& rains) {
        int n = rains.size();
        vector<int> ans (n, -1);
        unordered_map<int, int> f;
        priority_queue<int, vector<int>, greater<int>> q;
        for (int i = 0; i < n; ++i) {
            if (rains[i] > 0) {
                if (f.count(rains[i])) {
                    vector<int> tmp;
                    while (!q.empty() && q.top() < f[rains[i]]) {
                        tmp.push_back(q.top());
                        q.pop();
                    }
                    if (q.empty()) return {};
                    ans[q.top()] = rains[i];
                    q.pop();
                    for (auto x : tmp) q.push(x);
                }
                f[rains[i]] = i;
            } else {
                // cout << i << endl;
                q.push(i);
            }
        }
        while (!q.empty()) {
            ans[q.top()] = 1;
            q.pop();
        }
        return ans;
    }
};
```