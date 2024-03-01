题目描述：

![image](/algorithmn/dynamic_programming/image/image65.png)

解决过程：

没做出来

- 状态压缩、子集枚举

代码：

```cpp
class Solution {
public:
    int minNumberOfSemesters(int n, vector<vector<int>>& relations, int k) {
        vector<int> dp (1 << n, INT_MAX);
        vector<int> need(1 << n, 0);
        dp[0] = 0;
        for (const auto& relation : relations) {
            int pre = relation[0], suff = relation[1];
            need[(1 << (suff - 1))] |= (1 << (pre - 1));
        }
        for (int i = 1; i < (1 << n); i++) {
            need[i] = need[i & (i - 1)] | need[i & (-i)]; // 更新need[i]
            if ((need[i] | i) != i) {
                // 在计算dp[i]时，通过枚举i的子集sub，如果sub可以在本学期完成
                // 那么就可以得到dp[i] = dp[i ^ sub] + 1
                // 若need[i]不是i的子集，则在枚举i的子集的过程中，无法得到有效的sub
                // 即i仍然有课程未完成
                continue;
            }
            int valid = i ^ need[i]; // 枚举的是i中除去need[i]的课程的子集，因为need[i]不能最后进行
            if (__builtin_popcount(valid) <= k) {
                // 本学期可以直接完成valid
                dp[i] = min(dp[i], dp[i ^ valid] + 1);
                // 不需要担心dp[i ^ valid]为INT_MAX：
                // i - valid在之前已经遍历过，而且need[i]都是前置课程，对于dp[need[i]]而言，
                // 有两种可能：
                // 1. need[i]中所有课程都是入度为0的前置课程
                // 2. need[i]中仅有部分课程是入度为0的前置课程
                // 不论是哪种情况，need[need[i]] | need[i]都一定等于need[i]，因为一门课程入度不为0的
                // 前置课程的前置课程也是该门课程的前置课程
                // 因此dp[need[i]]一定是有效的
            } else {
                for (int sub = valid; sub; sub = (sub - 1) & valid) {
                    // 遍历valid的子集sub
                    if (__builtin_popcount(sub) <= k) {
                        // 这里的dp[i ^ sub]也一定是有效的：i ^ sub中包含了need[i]，need[i]具有i ^ sub中那些
                        // 非前置课程的前置课程，因此一定被计算过而且有效
                        dp[i] = min(dp[i], dp[i ^ sub] + 1);
                    }
                }
            }
        }
        return dp[(1 << n) - 1];
    } 
};
```