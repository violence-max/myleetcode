题目描述：  
![image](/basical/string/image/image12.png)  
解题过程：  
根本不会。  
看了题解，有三种方法，动态规划，中心扩展和Manacher算法。  
先说动态规划吧，也是我真正自己动手去做的一个方法。主要就是利用边界条件，即字符串长度为1以及2的情况进行一个长度从小到大的扩展。列出状态转移方程是最重要的。先遍历长度，再遍历左边界，每次遍历左边界都设置一个右边的右指针，这个右指针的索引大小减去左边界的长度刚好是要遍历的长度大小，合理地设置使得右指针不超过字符串的长度。如果做右指针相等，且其之差小于三，注意，这里包含了字符串长度为奇数或者偶数的两种情况，且左右指针之间的字符串长度最大，则设置左右指针之间的子串为最长字符串。  
再说说中心扩展吧。中心扩展就是以边界条件为基准，逐步向外扩展。遍历整个字符串，取左右指针相等以及右指针比左指针大1的情况，若左右指针的值相等则继续向外扩展，否则判断长度是否最大。  
最后来个Manacher算法，时间复杂度为O(1)的算法，极度离谱。  
代码：  
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int len = s.size();
        if (len < 2) return s;
        int maxlen = 1,result = 0,j; 
        vector<vector<bool>> dp (len,vector<bool> (len,false));
        for (int i = 0; i < len; i++) {
            dp[i][i] = true;
        }
        for (int L = 2; L <= len; L++) {
            for (int i = 0; i <= len - L; i++) {
                j = L + i - 1;
                if (s[j] == s[i]) {
                    if (j - i < 3) dp[i][j] = true;
                    else dp[i][j] = dp[i+1][j-1];
                    if (dp[i][j] && j - i + 1 > maxlen) {
                        result = i; maxlen = j - i + 1;
                    }
                }
            }
        }
        return s.substr(result,maxlen);
    }
};
```