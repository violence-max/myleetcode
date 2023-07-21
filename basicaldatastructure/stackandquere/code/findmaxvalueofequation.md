题目描述：

![image](/basicaldatastructure/stackandquere/image/image30.png)

解题过程：

我双端队列里面压的是下标不是y-x，就没做出来

代码：

```cpp
class Solution {
public:
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) {
        int ans = INT_MIN;
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> q;
        for (auto& p : points) {
            int x = p[0], y = p[1];
            while (!q.empty() && x - q.top().second > k) {
                q.pop();
            }
            if (!q.empty()) {
                ans = max(ans, y + x - q.top().first);
            }
            q.push({x - y, x});
        }
        return ans;
    }
};
```

```cpp
class Solution {
public:
    using pii = pair<int, int>;
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) {
        int res = INT_MIN;
        deque<pii> qu;
        for (auto &point : points) {
            int x = point[0], y = point[1];
            while (!qu.empty() && x - qu.front().second > k) {
                qu.pop_front();
            }
            if (!qu.empty()) {
                res = max(res, x + y + qu.front().first);
            }
            while (!qu.empty() && y - x >= qu.back().first) {
                qu.pop_back();
            }
            qu.emplace_back(y - x, x);
        }
        return res;
    }
};
```