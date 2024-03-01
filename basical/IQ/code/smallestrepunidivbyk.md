题目描述：

![image](/basical/IQ/image/image44.png)

解题过程：

没写出来

- 对1的长度进行不断增长来尝试是否能模k
- k如果与10互质则一定有解

代码：

```cpp
class Solution {
public:
    int smallestRepunitDivByK(int k) {
        if (k % 2 == 0 || k % 5 == 0) {
            return -1;
        }
        int ans = 1;
        int res = 1 % k;
        // cout << res << ' ' << ans << endl;
        while (res > 0) {
            res = (res * 10 + 1) % k;
            ans++;
            // cout << res << ' ' << ans << endl;
        }
        return ans;
    }
};
```