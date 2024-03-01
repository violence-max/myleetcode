题目描述：

![image](/basical/array/image/image81.png)

解决过程：

2分钟

代码：

```cpp
class Solution {
public:
    int maximumValue(vector<string>& strs) {
        int mx = 0;
        for (const auto& str : strs) {
            bool d = true;
            for (auto i : str) {
                if (!isdigit(i)) {
                    d = false;
                    break;
                }
            }
            int val = d ? stoi(str) : str.size();
            mx = max(mx, val);
        }
        return mx;
    }
};
```