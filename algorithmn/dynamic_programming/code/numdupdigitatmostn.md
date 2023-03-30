题目描述：  
![image](/algorithmn/dynamic_programming/image/image53.png)  
解决过程：  
每做出来  
- 数位dp
1. **从反面思考**，找出区间[0,n]之间没有重复的数字的个数即可
2. 从高位开始对可能的数字进行填充：假设我们填充第i位，在第i位上数字n对应的个位数为t
    1. 用布尔变量same来表示在填充第i位之前已经填充的数字是否和n对应位置上的数字上一一相等，即前缀相等
    2. 如果same为true，则在第i位上的选择位0~t
    3. 如果same位false，则在第i位上的选择为0~9
3. 由于我们要对不同位之间的数字是否相等做判断，而n最多有10位，因此可以用掩码mask表示在一个字符串中每一位的使用情况，如果已经有某个数字使用过了就可以直接跳过这个数字
4. 由此衍生除了动态规划的定义：dp[i][mask]表示的是在填充第i位且掩码为mask的情况下使得整个数字每位的数字互不相同的数字个数
5. 代码细节：
    1. 当遍历的位置来到个位数之后说明我们找到了一个可行的数字，这是答案加1
    2. 只有在前缀不相等的情况下我们的记忆化搜索才有效，即我们记录的dp[i][mask]才可以被复用，因为我们不会遇到两次前缀相等的情况
- 组合数学  
代码：（数位dp）  
```cpp
class Solution {
public:
    vector<vector<int>> dp;

    int f(int mask, const string& sn, int i, bool same) {
        if (i == sn.size()) {
            return 1;
        }
        if (!same && dp[i][mask] >= 0) {
            return dp[i][mask];
        }
        int res = 0, t = same ? (sn[i] - '0') : 9;
        for (int k = 0; k <= t; k++) {
            if (mask & (1 << k)) {
                continue;
            }
            res += f(mask == 0 && k == 0 ? mask : mask | (1 << k), sn, i + 1, same && k == t);
        }
        if (!same) {
            dp[i][mask] = res;
        }
        return res;
    }

    int numDupDigitsAtMostN(int n) {
        string sn = to_string(n);
        dp.resize(sn.size(), vector<int> (1 << 10, -1));
        return n + 1 - f(0,sn,0,true);
    }
};
```