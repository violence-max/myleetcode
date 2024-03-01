题目描述：

![image](/basical/string/image/image73.png)

解决过程：

竞赛题，ac

代码：

```cpp
class Solution {
public:
    int minimizedStringLength(string s) {
        unordered_map<char, int> map;
        int n = s.size();
        for (int i = 0; i < n; i++) {
            map[s[i]]++;
        }
        return map.size();
    }
};
```