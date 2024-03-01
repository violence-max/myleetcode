题目描述：

![image](/basical/string/image/image74.png)

解决过程：

看数据量太小暴力解了，看了题解发现后缀和更加棒

代码：

```cpp
class Solution {
public:
    vector<int> numSmallerByFrequency(vector<string>& queries, vector<string>& words) {
       function<int(string)> get = [&](const string& str) -> int {
           unordered_map<int, int> cnt;
           int mmin = 26;
           for (auto c : str) {
               cnt[c - 'a']++;
               if (c - 'a' < mmin) {
                   mmin = c - 'a';
               }
           }
           return cnt[mmin];
       };

       int n = words.size();
       vector<int> memo (12 , 0);
       int m = queries.size();
       vector<int> ans (m);
       
       for (int i = 0; i < n; ++i) {
           memo[get(words[i])]++;
       }
        for (int i = 11; i >= 0; --i) {
            memo[i] = i == 11 ? memo[i] : memo[i] + memo[i + 1];
        }
       for (int i = 0; i < m; ++i) {
           ans[i] = memo[get(queries[i]) + 1];
       }
       return ans;
     }
};
```