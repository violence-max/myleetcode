题目描述：

![image](/basical/IQ/image/image53.png)

解决过程：
没做出来

- 数论题

代码：

```cpp
class Solution {
public:
    using ll = long long;
    const int mod = 1e9 + 7;
    int minNonZeroProduct(int p) {
        long long x = (1LL << p) - 1;
        long long y = (1LL << p) - 2;
        long long n = (1LL << (p - 1)) - 1;
        long long res = 1;
        y = y % mod;
        while (n) {
            res = (long long)res * y % mod;
            y = (long long)y * y % mod;
            n = n >> 1;
        }
        return (res * (x % mod)) % mod;
    }
};
```