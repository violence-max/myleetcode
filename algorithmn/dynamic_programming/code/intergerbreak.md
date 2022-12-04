题目描述： 
![image](/algorithmn/dynamic_programming/image/image7.png)  
解题过程：  
这个题之前做过的，就是用一个基本不等式的证明，然后用快速幂去实现时间复杂度的降低。写的时候快速幂记得不清楚了，翻了一下笔记，就是每次底数取平方，遇到指数为奇数则结果乘一次底数，注意因为最后一次（指数为1）时底数会再取一次平方，因此底数最好用long型来赋值。写循环的时候我又忘记写判断变量的减小了，或者说写错了，pos >> 1，我去了。这个题和之前做过的那一道还有一点点不一样，这个题至少要拆分成两个正整数，所以要在小于等于4的部分做单独讨论。 
代码：　  
```cpp
class Solution {
public:
    int integerBreak(int n) {
        if (n == 2) return 1;
        if (n == 3) return 2;
        int mod = n % 3;
        int pos = n / 3;
        int ret = 1;
        long a = 3;
        while (pos) {
            if (pos & 1) {
                ret *= a;
            }
            a *= a;
            pos >>= 1;
        }
        return (mod == 0) ? ret : (mod == 1) ? (ret / 3) * 4 : ret * 2;
    }
};
```  
动态规划：靠，在写的时候发生内存溢出的问题，居然没有排查出来，看了题解才发现，自己的dp数组少了一个单元，应该多分配一个才对，因为要直接访问最后一个位置，而不是索引的n-1，看来数组的越界问题很重要啊！  
```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n+1,0);
        for (int i = 2; i <= n; i++) {
            int curmax = 0;
            for (int j = 1; j < i; j++) {
                curmax = max(curmax,max(j*(i-j),j*dp[i-j])); 
            }
            dp[i] = curmax;
        }
        return dp[n];
    }
};
```