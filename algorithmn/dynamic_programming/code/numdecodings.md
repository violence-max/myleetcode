题目描述：  
![image](/algorithmn/dynamic_programming/image/image41.png)  
解决过程：  
一开始又用回溯，果断超时。感觉回溯是个笨办法，但凡时间复杂度太高的回溯以后都别用了。而且我在判断数据data是否在1到26之间时居然蠢到用了bool表达式1 ≤ data ≤ 26，无敌了  
看了题解，一看到动态规划这几个字我直接恍然大悟，看来方法的意识还是太弱了。我甚至觉得这个题不需要细致地描述算法，因为没必要记，重要的是方法。如果我一开始知道要用动态规划去做这个题，三下五除二我就得到答案了。还有一个很重要地一点是空间的节省，一般的动态规划问题可能都会用到dp数组，如果能把dp数组换位几个常量，那就无敌了  
实现常数动规的过程比较波折，有两个错误提交。其实在考虑三个状态before，pre和ans的过程中，应该知道的是每次循环需要求解的是ans，而ans只有在当前循环可以选择单个数进行解码映射时才会等于pre，因此每次循环时ans的初值都是0！而before和pre因为在没进入for循环之前就需要定义，而在没进入for循环之前before和pre都指向了空数组的位置，因此按照定义before和pre只有一种映射情况—什么都不做，因此应该赋初值为1，这样就很好理解了  
代码：（数组动规→常数动规）  
```cpp
class Solution {
public:
    int numDecodings(string s) {
        int ls = s.length();
        vector<int> dp (ls + 1);
        dp[0] = 1;
        for (int i = 1; i <= ls; i++) {
            if (s[i-1] != '0') {
                dp[i] = (i == 1) ? 1 : dp[i-1];
            }
            if (i > 1 && s[i-2] != '0') {
                int data = (s[i-1] - '0') + (s[i-2] - '0') * 10;
                if (1 <= data && data <= 26) {
                    dp[i] += dp[i-2];
                }
            }
        }
        return dp[ls];
    }
};
```
```cpp
class Solution {
public:
    int numDecodings(string s) {
        int ls = s.length();
        int before = 1,pre = 1,ans;
        for (int i = 0; i < ls; i++) {
            ans = 0;
            if (s[i] != '0') {
                ans = pre;
            }
            if (i > 0 && s[i-1] != '0') {
                int data = (s[i] - '0') + (s[i-1] - '0') * 10;
                if (1 <= data && data <= 26) {
                    ans += before;
                }
            }
            before = pre;
            pre = ans;
        }
        return ans;
    }
};
```