题目描述：

![image](/basicaldatastructure/binary_tree/image/image63.png)

解决过程：

第355场周赛t4，没做出来

- 异或法则

代码：

```cpp
class Solution {
public:
    long long countPalindromePaths(vector<int>& parent, string s) {
        int n = parent.size();
        vector<pair<int,int>> d[n];

        for (int i = 1; i < n; ++i) {
            int bit = 1 << (s[i] - 'a');
            d[parent[i]].push_back({i, bit});
        }
        long long ans = 0;
        unordered_map<int,long long> ss;
        function<void(int,int)> dfs = [&](int i, int xxor) {
            ans += ss[xxor];

            for (int i = 0; i < 26; ++i) {
                ans += ss[(1 << i) ^ xxor];
            }
            ss[xxor]++;
            for (auto ni : d[i]) {
                dfs(ni.first, xxor ^ ni.second);
            }
        };
        dfs(0,0);
        return ans;
    }
};
```