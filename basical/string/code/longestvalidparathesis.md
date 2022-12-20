题目描述：  
![image](/basical/string/image/image23.png)  
解题过程：  
想用栈匹配的思想去解决这个问题，但是发现没有办法判断是否为连续子串，就看了答案。  
答案提供了三种方法：动态规划、栈存储和两次遍历  
动态规划的解法应该是比较难理解的。dp[i]存储的是以下标i为结尾的子串的最长有效括号匹配长度，状态方程最难的就是在出现第i-1个字符也是’)’的情况，这种情况必须判断在前一个位置的最长有效括号子串之前的一个字符是否为’(’，如果是不仅仅要加上2，还要加上在这个字符之前的最长有效括号子串的长度  
栈存储的方法比较通俗易懂。栈存储的是左括号或者右括号的下标，栈底永远存储的是**最后一个没有被匹配的右括号**，这样子才能判断连续的子串。通过索引相减的方法来解决问题。  
两次遍历就是从前往后一次和从后往前一次。每次都记录左括号和右括号的个数，若相等则记录答案，若发生遍历方向的最后一个字符的出现次数比第一个要大的情况则将两个字符的个数清零（很细节）。至于为什么可以通过字符的个数来判断是否为有效最长括号，原因是有效的括号只有两种可能：无限包含和紧邻或者两者结合，这几种情况都满足个数左右括号个数相等的情况（当然）。而且个数归零的规则也确保了是子串，再加上两个方向的遍历，强无敌。 
代码：  
两次遍历： 
```cpp
class Solution {
public:
    int traverse(string s,bool forward) {
        int left = 0, right = 0,ls = s.size();
        int res = 0;
        if (!forward) reverse(s.begin(),s.end());
        for (int i = 0; i < ls; i++) {
            if (s[i] == '(')
                left++;
            else if (s[i] == ')')
                right++;
            if (left == right) {
                res = max(res,right*2);
            } else if ((forward && right > left) || (!forward && left > right)) {
                left = right = 0;
            }
        }
        return res;
    } 
    int longestValidParentheses(string s) {
        return max(traverse(s,true),traverse(s,false));
    }
};
```  
动态规划：  
```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        int maxans = 0, n = s.length();
        vector<int> dp(n, 0);
        for (int i = 1; i < n; i++) {
            if (s[i] == ')') {
                if (s[i - 1] == '(') {
                    dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
                } else if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] == '(') {
                    dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
                }
                maxans = max(maxans, dp[i]);
            }
        }
        return maxans;
    }
};
```  
栈存储：  
```cpp
class Solution {
    public int longestValidParentheses(String s) {
        int maxans = 0;
        Deque<Integer> stack = new LinkedList<Integer>();
        stack.push(-1);
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.isEmpty()) {
                    stack.push(i);
                } else {
                    maxans = Math.max(maxans, i - stack.peek());
                }
            }
        }
        return maxans;
    }
}
```