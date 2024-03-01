题目描述：

![image](/basical/string/image/image87.png)

解决过程：

没做出来，没有想明白复杂度和数据范围

代码：

```cpp
class Solution {
public:
    vector<vector<int>> substringXorQueries(string s, vector<vector<int>>& queries) {
        unordered_map<string, pair<int,int>> map;
        int n = s.size();
        int zpos = -1;
        for (int i = 0; i < n; ++i) {
            if (s[i] == '0') {
                if (zpos == -1) {
                    zpos = i;
                }
                continue;
            }
            string str = "";
            for (int j = i; j < n && j < i + 30; ++j) { // j的上界很重要
                str += s[j];
                if (!map.count(str)) {
                    map[str] = make_pair(i, j);
                }
            }
        }
        n = queries.size();
        vector<vector<int>> ans (n);
        for (int i = 0; i < n; ++i) {
            int val = queries[i][0] ^ queries[i][1];
            if (val == 0) {
                ans[i] = {zpos,zpos};
                continue;
            }
            string str = "";
            while (val) {
                str += '0' + (val & 1);
                val = val >> 1;
            }
            reverse(str.begin(), str.end());
            // cout << i << ' ' << str << endl;
            if (map.count(str)) {
                ans[i] = {map[str].first, map[str].second};
            } else {
                ans[i] = {-1,-1};
            }
        }
        return ans;
    }
};
```