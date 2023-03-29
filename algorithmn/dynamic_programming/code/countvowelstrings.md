题目描述：  
![image](/algorithmn/dynamic_programming/image/image52.png)  
解决过程：  
每做出来：  
- 动态规划：
1. 假设分别以数字0、1、2、3、4代表元音a、e、i、o、u
2. 设dp[i][j]表示长度为i+1，以字符j结尾的符合条件的字符串数量
3. 状态转移方程  
$$
dp[i][j] = 
\begin{cases}
1 & , i == 0 \\
\sum_{k=0}^jdp[i-1][k] & , i > 0
\end{cases}
$$
- 组合数学：
1. 相当于将n个球放进5个篮子，并且允许有空蓝
2. 可以转化为将n + 5个球放进5个篮子，多出来的5相当于是虚拟出来的5个球
3. 所以当长度为n时可以直接得到答案长度为：$C_{n+4}^{4}$  
代码：（动态规划→组合数学）  
```cpp
class Solution {
public:
    int countVowelStrings(int n) {
        vector<int> dp (5, 1);
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < 5; j++) {
                dp[j] += dp[j - 1];
            }
        }
        return accumulate(dp.begin(), dp.end(), 0);
    }
};
```  
```cpp
class Solution {
public:
    int countVowelStrings(int n) {
        return (n + 1) * (n + 2) * (n + 3) * (n + 4) / 24;
    }
};
```