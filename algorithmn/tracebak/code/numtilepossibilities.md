题目描述：

![image](/algorithmn/tracebak/image/image27.png)

解决过程：

没做出来，在全排列的地方纠结了很久

- 回溯

代码：

```cpp
class Solution {
public:
    int numTilePossibilities(string tiles) {
        unordered_map<char, int> fre;
        set<char> s;
        for (auto c : tiles) {
            fre[c]++;
            s.emplace(c);
        }
        return dfs(fre, s, tiles.size()) - 1;
    }

    int dfs(unordered_map<char, int>& fre, const set<char>& s, int length) {
        if (length == 0) {
            return 1; // 空字符串默认可以排出一个序列
        }

        int res = 1; // 减去空字符串的那个序列
        for (auto c : s) {
            if (fre[c] > 0) {
                // 对所有可以使用的字符遍历，如果还有留存（频率大于0）则取出
                
                fre[c]--;
                res += dfs(fre, s, length - 1);
                fre[c]++;
            }
        }

        return res;
    }
};
```