题目描述：

![image](/basical/simulation/image/image2.png)

解决过程：

30sac

代码：

```cpp
class Solution {
public:
    int alternateDigitSum(int n) {
        string s = to_string(n);
        int x = s.size();
        int ans = 0;
        int temp = 1;
        for (int i = 0; i < x; ++i) {
            ans += temp * (s[i] - '0');
            temp *= -1;
        }
        return ans;
    }
};
```