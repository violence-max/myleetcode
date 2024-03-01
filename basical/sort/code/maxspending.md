题目描述：

![image](/basical/sort/image/image5.png)

解决过程：

第117场双周赛t4，没做出来

代码：

```cpp
class Solution {
public:
    long long maxSpending(vector<vector<int>>& values) {
        int m = values.size(), n = values[0].size();
        vector<int> a;
        a.reserve(m * n);
        for (auto& row : values) {
            a.insert(a.end(), row.begin(), row.end());
        }
        sort(a.begin(), a.end());
        long long ans = 0;
        for (int i = 0; i < m * n; ++i) {
            cout << i << ' ' << a[i] << endl;
            ans += (long long)a[i] * (i + 1);
        }
        return ans;
    }
};
```