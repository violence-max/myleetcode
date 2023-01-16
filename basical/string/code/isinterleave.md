题目描述：  
![image](/basical/string/image/image42.png)  
解决过程：  
这道题思路想对了，动态规划，就是动态规划方程想不出来啊。我根本没有想到考虑s3的最后一个字符串是来自s1就好还是s2就好，我靠，实在是太丝滑了这个思路。题解的代码实现也很赞，我太喜欢那种把很多种情况统一起来放到几行代码里面实现的东西了。  
代码：（二维dp数组→一维dp数组）  
```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int ls1 = s1.length(),ls2 = s2.length(),ls3 = s3.length();
        if (ls1 + ls2 != ls3) return false;
        vector<vector<bool>> dp (ls1+1,vector<bool> (ls2+1));
        dp[0][0] = true;
        for (int i = 0; i <= ls1; i++) {
            for (int j = 0; j <= ls2; j++) {
                int k = i + j - 1;
                if (i > 0) {
                    dp[i][j] = dp[i][j] | (s1[i-1] == s3[k] && dp[i-1][j]);
                }
                if (j > 0) {
                    dp[i][j] = dp[i][j] | (s2[j-1] == s3[k] && dp[i][j-1]);
                }
            }
        }
        return dp[ls1][ls2];
    }
};
```
```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int ls1 = s1.length(),ls2 = s2.length(),ls3 = s3.length();
        if (ls1 + ls2 != ls3) return false;
        vector<bool> dp (ls2+1);
        dp[0] = true;
        for (int i = 0; i <= ls1; i++) {
            for (int j = 0; j <= ls2; j++) {
                int k = i + j - 1;
                if (i > 0) {
                    dp[j] = (s1[i-1] == s3[k]) & dp[j];
                }
                if (j > 0) {
                    dp[j] = dp[j] | (s2[j-1] == s3[k] & dp[j-1]);
                }
            }
        }
        return dp[ls2];
    }
};
```