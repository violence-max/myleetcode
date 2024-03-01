题目描述：

![image](/basical/IQ/image/image48.png)

解决过程：

第351场周赛t2，写了一个多小时写不出来

代码：

```cpp
class Solution {
public:
    int makeTheIntegerZero(int num1, int num2) {
        for (int k = 1; k < 62; ++k) { // 由于x的最大值中1的个数不会太大，这里把k枚举到61已经很超过了
            // 枚举操作次数
            // 如果x可以用k个2的幂相加得到，即找到符合条件的最小的操作次数
            long long x = (long long)num1 - (long long)num2 * k;
            if (x < k || x < 0) {
                // 如果num2 < 0, 又num1 >= 1，因此x >= k
                // 如果num2 > 0，则可能出现这种情况，并且随着k的增大x < k恒成立
                break;
            }
            // x最多可以使用x.bit_count()个2的幂相加得到，最多使用x个1相加得到
            // 当k >= x.bit_count() && k <= x时，x可以使用k个2的幂相加得到
            if (k >= __builtin_popcountll(x)) {
                return k;
            }
        }
        return -1;
    }
};
```