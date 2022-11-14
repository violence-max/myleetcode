题目描述：  
![image](/basicaldatastructure/stackandquere/image/image5.png)  
解题过程：  
这个题是寂寞的，我看了第一眼根本不知道怎么快速写出好吧。因为这个题的归类属于栈和队列的类型，所以必须要用栈和队列的知识去解决这个问题，那么我该怎么去解决这个问题呢？根本不知道。因为我觉得有效的括号肯定会包括无数的括号嵌套循环的情况，但是我又不知道怎么去解决这个情况。哎，看了题解，感觉解法太无敌了，原来就是用一个栈和一个哈希表去辅助，所有括号的左部分入栈，如果遇到括号的右部分，栈顶的元素必然是对应括号的左部分，我超，这个想法简直无敌。  
除此之外，还有一些小地方，比如哈希表怎么建立啊，什么条件下返回false，什么条件下返回true啊，太无敌了。  
代码：  
```cpp
class Solution {
private:
    stack<char> Stack;
    unordered_map<char,char> pairs = {
        {')','('},
        {'}','{'},
        {']','['}
    };
public:
    bool isValid(string s) {
        if (s.size() & 1 == 1) return false;
        for (char c : s) {
            if (pairs.count(c)) {
                if (Stack.empty() || Stack.top() != pairs[c]) {
                    return false;
                } else {
                    Stack.pop();
                }
            } else {
                Stack.push(c);
            }
        }
        if (Stack.empty()) return true;
        else return false;
    }
};
```