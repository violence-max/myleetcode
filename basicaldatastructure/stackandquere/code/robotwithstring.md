题目描述：

![image](/basicaldatastructure/stackandquere/code/robotwithstring.md)

解决过程：

没做出来，没有想清楚最优解的证明

代码：

```cpp
class Solution {
public:
    string robotWithString(string s) {
        string ans = "";
        int cnt[26]{}, min = 0;
        for (auto c : s) ++cnt[c-'a'];
        stack<char> stk;
        for (auto c : s) {
            --cnt[c-'a'];
            while (min < 25 && cnt[min] == 0) ++min;
            stk.push(c);
            while (!stk.empty() && stk.top() - 'a' <= min) { // 只要栈顶的字符小于等于后续的最小字符，那么使用当前栈顶字符就是最优解
                ans += stk.top();
                stk.pop();
            }
        }
        return ans;
    }
};
```