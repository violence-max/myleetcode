题目描述：  
![image](/basicaldatastructure/stackandquere/image/image6.png)  
解题过程：  
这个题和括号匹配几乎一模一样，就是把字符压入栈中，然后栈顶如果是一样的字符就删除就ok了。  
代码：  
```cpp
class Solution {
private:
    stack<char> Stack;
public:
    string removeDuplicates(string s) {
        if (s.size() == 0) return s;
        for (char c : s) {
            if (!Stack.empty() && Stack.top() == c) {
                Stack.pop();
            } else {
                Stack.push(c);
            }
        }
        string result;
        while (!Stack.empty()) {
            result.insert(result.begin(),Stack.top());
            Stack.pop();
        }
        return result;
    }
};
```