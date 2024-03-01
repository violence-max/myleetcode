题目描述：

![image](/basical/array/image/image95.png)

解决过程：

没做出来，差分+前缀和

代码：

```cpp
class Solution {
public:
    vector<vector<long long>> splitPainting(vector<vector<int>>& segments) {
        unordered_map<int, long long> s;
        for (const auto& v : segments) {
            s[v[0]] += v[2];
            s[v[1]] -= v[2];
        }
        vector<pair<int, long long>> a;
        for (const auto& x : s) {
            a.push_back({x.first, x.second});
        }
        sort(a.begin(), a.end());
        int n = a.size();
        for (int i = 1; i < n; ++i) {
            // 区间只可能在端点处形成，所以排序后可以直接考虑每个点的区间颜色
            // 差分来考虑重复部分
            a[i].second += a[i-1].second;
        }
        vector<vector<long long>> b;
        for (int i = 0; i < n - 1; ++i) {
            if (a[i].second) // 有点坑
                b.push_back(vector<long long>{a[i].first, a[i+1].first, a[i].second});
        }
        return b;
    }
};
```