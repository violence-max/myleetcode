题目描述：  
![image](/basical/string/image/image60.png)  
写了挺久的，挺久没做题了，现在找感觉  
我用指针写思来想去写不出来，于是用栈保存了没有删除的字母b。主要思想是为了b不能出现在a的左边，但凡遇到a而且这个a之前还存在没有删除的b，答案就加1  
但是题解里有一个code友的思路就很好：  
- 动态规划：dp[i]表示0~i需要删除的最少次数，则当第i个字符为b时dp[i]不变，表示保留左侧的b；当第i哥字符为a时选择删除该字符或者删除a左侧所有的b  
$$
    dp[i] = 
    \begin{cases}  
    dp[i-1], s[i] = 'b'  \\
    min(sumb, dp[i-1] + 1), s[i] = 'a'
    \end{cases}
$$
代码：（栈→动态规划）  
```cpp
class Solution {
public:
    int minimumDeletions(string s) {
        int ls = s.length();
        int res = 0, ptr = 0;
        stack<int> stk;
        while (ptr < ls) {
            if (s[ptr] == 'a' && !stk.empty() && ptr > stk.top()) {
                res++;
                stk.pop();
            } else if (s[ptr] == 'b') {
                stk.push(ptr);
            }
            ptr++;
        }
        return res;
    }
};
```
```cpp
class Solution {
public:
    int minimumDeletions(string s) {
        int dp = 0, ptr = 0, sumb = 0;
        while (ptr < s.length()) {
            if (s[ptr] == 'b') sumb++;
            else {
                dp = min(dp + 1, sumb);
            } 
            ptr++;
        }
        return dp;
    }
};
```