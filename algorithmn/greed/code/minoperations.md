题目描述：

![image](/algorithmn/greed/image/image26.png)

解决过程：

第360场周赛t3，没做出来。

从低位到高位判断是否能凑出target的第i位，如果不能，寻找最近的一位进行分裂操作

代码：

```cpp
class Solution {
public:
    int c[32], sum[32];
    int minOperations(vector<int>& nums, int target) {
        c[0] = 1;
        for (int i = 1; i < 32; ++i) {
            c[i] = c[i-1] << 1;
        }
        memset(sum, 0, sizeof sum);
        for (int n : nums) {
            for (int i = 0; i < 32; ++i) {
                if (n == c[i]) {
                    sum[i]++;
                }
            }
        }

        long long nows = 0;
        int ans = 0;
        for (int i = 0; i < 32; ++i) {
            nows += (long long)sum[i] * c[i];
            if (target & c[i]) {
                if (nows >= c[i]) {
                    nows -= c[i];
                } else {
                    int aim = -1;
                    for (int j = 31; j > i; --j) {
                        if (sum[j] > 0) aim = j;
                    }
                    if (aim == -1) return -1;
                    for (int k = aim - 1; k > i; --k) {
                        ans++;
                        sum[k]++;
                    }
                    ans++;
                    --sum[aim];
                    nows += c[i];
                }
            }
        }
        return ans;
    }
};
```