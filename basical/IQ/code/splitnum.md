题目描述：

![image](/basical/IQ/image/image57.png)

解决过程：

没做出来

考察到了单调递增和对比构造的性质

代码：

```cpp
class Solution {
public:
    int splitNum(int num) {
        string str = to_string(num);
        sort(str.begin(), str.end());
        int num1 = 0, num2 = 0;
        for (int i = 0; i < str.size(); ++i) {
            if (i % 2 == 0) {
                num1 = num1 * 10 + (str[i] - '0');
            } else {
                num2 = num2 * 10 + (str[i] - '0');
            }
        }
        return num1 + num2;
    }
};
```