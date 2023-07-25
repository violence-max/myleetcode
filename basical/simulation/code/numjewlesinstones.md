题目描述：

![image](/basical/simulation/image/image12.png)

解决过程：

模拟

代码：

```
class Solution {
public:
    int numJewelsInStones(string jewels, string stones) {
        unordered_set<char> s;
        for (auto c : jewels) {
            s.insert(c);
        }
        int res = 0;
        for (auto c : stones) {
            res += s.count(c);
        }
        return res;
    }
};
```