题目描述：  
![image](/algorithmn/dynamic_programming/image/image17.png)  
解题过程：  
这道题，一开始我还想着是写一个判断子串的函数来着，但是想着好麻烦啊，就没血了，时间也过去挺久了，就没有再继续写下去了。  
看了题解，第一眼感觉是枚举，再多看几眼＋自己复现了一波，感觉强无敌，虽然是一个时间复杂度为平方级别的暴力解法，但是动态规划方程的思想还是那么的抽象以及不可思议：dp[i] 标识待组成的字符串的前i个字符能够在字典中找到。然后借助巧妙地子串匹配（哈希），直接解决了字符串组成的问题，强无敌！  
代码：  
```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> set;
        for (string s : wordDict) {
            set.insert(s);
        }
        vector<bool> dp(s.size()+1);
        dp[0] = true;
        for (int i = 1; i <= s.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && set.find(s.substr(j,i-j)) != set.end()) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.size()];
    }
};
```