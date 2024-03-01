题目描述：

![image](/basical/array/image/image91.png)

解决过程：
没做出来

- 仿射变换

代码：

```cpp
class Solution {
public:
    unordered_map<int, vector<int>> m;
    vector<int> beautifulArray(int n) {
        if (m.count(n)) return m[n];
        vector<int> ans (n);
        if (n == 1) ans[0] = 1;
        else {
            int i = 0;
            for (const int& x : beautifulArray((n + 1) / 2)) {
                ans[i++] = 2 * x - 1; // 一个漂亮数组经过仿射变换之后仍然是漂亮数组，这里映射到奇数部分
            }
            for (const int& x : beautifulArray(n / 2)) {
                ans[i++] = 2 * x; // 映射到偶数
            }
        }
        return m[n] = ans;
    }
};
```