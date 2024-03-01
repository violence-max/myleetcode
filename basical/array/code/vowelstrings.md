题目描述：

![image](/basical/array/image/image74.png)

解决过程：

没做出来，时间复杂度超了

- 前缀和

代码：（前缀和）

```cpp
class Solution {
public:
    unordered_set<char> set = {'a', 'e', 'i', 'o', 'u'};

    int check(const string& str) {
        return ( set.count(str[0]) && set.count(str[str.size()-1]) ) ? 1 : 0;
    }

    vector<int> vowelStrings(vector<string>& words, vector<vector<int>>& queries) {
        int n = words.size();
        vector<int> prev (n);
        for (int i = 0; i < n; i++) {
            int val = check(words[i]);
            prev[i] = i == 0 ? check(words[i]) : prev[i-1] + check(words[i]);
        }    

        int m = queries.size();
        vector<int> ans (m);
        for (int i = 0; i < m; i++) {
            int left = queries[i][0], right = queries[i][1];
            ans[i] = prev[right] - (left > 0 ? prev[left-1] : 0);
        }

        return ans;
    }
};
```