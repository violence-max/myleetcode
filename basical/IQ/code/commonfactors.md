题目描述：  
![image](/basical/IQ/image/image34.png)  
解题过程：  
我是枚举到最小值  
- 题解：枚举到最大公因数
1. x是a和b的公因子当且仅当x是gcd(a,b)的因子
2. 设c是a和b的最大公因子，由于因子是成对出现的，所以可以枚举区间$[1,\sqrt{c}]$：如果i是c的因子则答案加1，如果i * i ≠ c则答案再次加1  
代码：  
```cpp
class Solution {
public:
    int commonFactors(int a, int b) {
        int c = gcd(a, b), ans = 0;
        for (int x = 1; x * x <= c; ++x) {
            if (c % x == 0) {
                ++ans;
                if (x * x != c) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```