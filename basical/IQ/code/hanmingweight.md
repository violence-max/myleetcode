题目描述：

![image](/basical/IQ/image/image45.png)

解决过程：

- 循环检查
- n & (n - 1) 消去最低位的1

代码：

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int bit = 0;
        int res = 0;
        while (bit < 32) {
            int _bit = 1 << bit;
            res += (n & _bit) > 0;
            // cout << bit << ' ' << res << endl;
            bit++;
        }
        return res;
    }
};
```