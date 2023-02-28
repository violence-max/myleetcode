题目描述：  
![image](/basical/string/image/image58.png)  
解决过程：  
我没考虑到中括号的嵌套，因此没写出来  
- 栈模拟：
    1. 遇到数字则将数字计算出来，如果ptr到达字符串s的末尾说明s是一个只包含数字的字符串，解码后为空字符串，否则将数字入栈
    2. 遇到字母或者左中括号直接入栈
    3. 遇到右中括号则在栈中不断弹出元素直到栈顶元素为左中括号为止，此时得到一个需要重复的字符串。把字符串重复相应的次数之后重新入栈，但是注意入栈之前需要把数字弹出
    4. 最后栈中剩下的就是解码后的字符串了  
代码：（栈）  
```cpp
class Solution {
public:
    string getString(const vector<string>& vec) {
        string res;
        for (auto& str : vec) {
            res += str;
        }
        return res;
    }

    string getDigits(const string& s, int& ptr) {
        string res;
        while (isdigit(s[ptr])) {
            res += s[ptr++];
        }
        return res;
    }

    string decodeString(string s) {
        int len = s.length();
        vector<string> stk;
        int ptr = 0;
        while (ptr < len) {
            if (isdigit(s[ptr])) {
                string number = getDigits(s,ptr);
                if (ptr == len) return "";
                stk.push_back(number);
                continue;
            } else if (isalpha(s[ptr]) || s[ptr] == '[') {
                string tmp;
                tmp += s[ptr];
                stk.push_back(tmp);
            } else {
                vector<string> sub;
                while (stk.back() != "[") {
                    sub.push_back(stk.back());
                    stk.pop_back();
                }
                stk.pop_back();
                reverse(sub.begin(),sub.end());
                int num = stoi(stk.back());
                stk.pop_back();
                string duplicate, tmp = getString(sub);
                while (num--) duplicate += tmp;
                stk.push_back(duplicate);                
            }
            ptr++;
        }
        return getString(stk);
    }
};
```