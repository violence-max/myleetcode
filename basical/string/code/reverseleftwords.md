题目描述：  
![image](/basical/string/image/image9.png)  
解题过程：  
这个题太简单了，直接把两个单独的字符串分开然后添加到答案里面就可以了。看了题解，有一种取余的操作很强，可以一次循环完成添加。题解还有字符串拼接等方法，都还行。我用c++写的，居然没有想到substr这个api。  
代码：  
```cpp
class Solution {
public:
    void pushString(string &res,string toBePushed) {
        for (char c : toBePushed) {
            res.push_back(c);
        }
    }
    string reverseLeftWords(string s, int n) {
        string fisrt,second;
        int i = 0,length = (int)s.size();
        while (i < n) {
            fisrt.push_back(s[i++]);
        }
        while (i < length) {
            second.push_back(s[i++]);
        }
        string res;
        pushString(res,second);
        pushString(res,fisrt);
        return res;
    }
};
```