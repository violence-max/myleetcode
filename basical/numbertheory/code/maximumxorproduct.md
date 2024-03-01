题目描述：

![image](/basical/numbertheory/image/image2.png)

解决过程：

第372场周赛，t3，没做出来

代码：

```cpp
class Solution {
public:
    int maximumXorProduct(long long a, long long b, int n) {
        if (a < b) { // 保证 a >= b
            swap(a, b);
        }

        long long mask = (1LL << n) - 1;
        long long ax = a & ~mask;
        long long bx = b & ~mask; // 高于n位的部分x无法影响
        a = a & mask;
        b = b & mask; // 低于n位的部分x可以影响

        long long left = a ^ b; // a和b中不同的位，x可以使其中一位为1，另一位为0
        long long one = mask ^ left; // x最终可以使得异或之后a和b为1的部分
        ax = ax | one;
        bx = bx | one;

        // 分配left
        if (left > 0 && ax == bx) {
            // 由于ax + bx是一个定值，因此应当分配使得ax和bx尽可能相等
            int hightbit = 63 - __builtin_clzll(left);
            ax = ax | (1LL << hightbit);
            left = left ^ (1LL << hightbit);
        }
        bx = bx | left; // 如果ax > bx则应当全部分给bx

        int mod = 1e9 + 7;
        return ax % mod * (bx % mod) % mod;
    }
};
```