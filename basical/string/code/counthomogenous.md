题目描述：  
![image](/basical/string/image/image30.png)  
憨批题目，就是统计连续子字符串的个数而已，然而，因为我在写1的9次方+7的时候又犯了位运算符号优先级比加号低的错误，我一度怀疑是不是对题目理解出错了，直到我看了题解才发现我的理解没有问题，然后某一次测试的时候leetcode的编译器提醒我位运算那个地方出错了我才知道原来犯了这么个错误。  
题解： 
```cpp
class Solution {
public:
    int countHomogenous(string s) {
        long long MOD = 1e9 + 7;
        long long ans = 0;
        int cnt = 0;
        char pre = s[0];
        for (auto c : s) {
            if (pre == c) {
                cnt++;
            } else {
                ans += (long long)(cnt + 1) * cnt / 2;
                cnt = 1;
                pre = c;
            }
        }
        ans += (long long)(cnt + 1) * cnt / 2;
        return ans % MOD;
    }
};
```  
自己：  
```cpp
class Solution {
public:
    int countHomogenous(string s) {
        long long MOD = 1e9 + 7;
        long long ans = 0;
        int start = 0,i = 0,ls = (int)s.size();
        for (int i = 0; i < ls; i++) {
            if (s[start] != s[i]) {
                ans += 1;
                start = i;
            } else {
                ans += i - start + 1;
            }
        }
        return ans % MOD;
    }
};
```