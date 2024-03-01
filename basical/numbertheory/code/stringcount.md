题目描述：

![image](/basical/numbertheory/image/image1.png)

解题过程：

第117场双周赛t3，没做出来

容斥原理，数论经典，真难则反

代码：

```cpp
class Solution {
public:
    int mod = 1e9 + 7;

    long long pow(long long x, int n) {
        long long res = 1;
        while (n) {
            if (n & 1) res = res * x % mod;
            x = x * x % mod;
            n = n >> 1;
        }
        return res;
    }

    int stringCount(int n) {
        return ((pow(26, n) - (75 + n) * pow(25, n - 1) + (72 + 2 * n) * pow(24, n - 1) - (23 + n) * pow(23, n - 1)) % mod + mod) % mod;    
    }
};
```