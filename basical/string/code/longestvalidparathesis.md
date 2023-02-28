题目描述：  
![image](/basical/string/image/image23.png)  
解题过程：  
想用栈匹配的思想去解决这个问题，但是发现没有办法判断是否为连续子串，就看了答案。  
答案提供了三种方法：动态规划、栈存储和两次遍历  
- 动态规划：
    1. dp[i]表示以下标i结尾的最长有效括号的长度
    2. 我们只需要以当s[i]为右括号时分类讨论即可：s[i-1]为左括号→dp[i] = dp[i-2] + 2；s[i-1]为右括号→如果dp[i-dp[i-1]-1]为左括号，则dp[i] = dp[i-dp[i-1]-2] + 2
- 栈存储：
    1. 栈存储的是下标
    2. 一开始栈会存入-1表示最后一个没有被匹配的右括号
    3. 当遇到左括号时入栈
    4. 遇到右括号时将栈顶元素弹出，如果栈为空说明弹出的元素是上一个为被匹配的右括号，则使用当前右括号的下标入栈；否则说明弹出的元素是一个可以与当前右括号匹配的左括号，使用当前右括号的下标i与**当前**栈顶元素相减得到一个有效括号长度
- 两次遍历：  
    以从左往右遍历为例  
    1. 用left和right分别记录左括号和右括号出现的次数
    2. 当left==right时则可以得到一个有效括号长度
    3. 当right > left时将两者置零（右括号出现在左括号之前的情况）  
    但是有一种情况处理不了：`”(()”` ，这种情况下最长有效括号长度应该是2，但是从左到右的遍历无法检测出来。因此需要两次遍历  
    一开始会在这个方法的正确性上纠结，不明白为什么通过左括号和右括号出现次数就可以断定最长有效括号长度。后来发现无论是哪种有效括号的组合，左括号总是在前，最后一个字符肯定是右括号，因此通过次数的判断是肯定可以确定最长有效括号的长度的。最难的地方应该是在处理左括号次数始终大于右括号次数的情况下，这时候需要反向遍历  
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