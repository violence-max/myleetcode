题目描述：  
![image](/basical/string/image/image1.png)
解题过程：  
这个题目，一开始看到，我认为应该写一个处理字符串的函数，然后直接比较处理后的字符串是否相等即可。但是我不知道c++能不能直接像python一样直接比较字符串，还是需要一个比较函数，后来我发现是可以直接比较的，可能跟有这么一个类型的变量有关吧。难题其实主要还是在如何处理字符串，我想的是遇到井号就把之前的字符给kill掉，然而这样好像有点困难，因为不知道c++的字符串特性等等，也不清楚相关api，走c语言最基础的指针复制应该不太可行感觉，所以就有点难。  
看了题解以后，发现居然可以用栈这个数据结构去解决，遇到井号就弹栈，真的强无敌。但是有进阶版，就是使用O(1)的时间复杂度，可惜了，暂时不会。  
代码：  
```cpp
class Solution {
public:
    string excute(string r) {
        string result;
        for (char ch : r) {
            if (ch != '#') {
                result.push_back(ch);
            } else {
                if (!result.empty()) {
                    result.pop_back();
                }
            }
        }
        return result;
    }
    
    bool backspaceCompare(string s, string t) {
        return excute(s) == excute(t);
    }
};
```