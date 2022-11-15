题目描述：  
![image](/basicaldatastructure/stackandquere/image/image7.png)  
解题过程：  
非常经典的一道栈题目，这道题思路非常简单，就是在遇到运算符之前把所有整数都放到栈里面，遇到运算符之后去除栈顶符号作为第二个操作数，弹出，再取出栈顶符号作为第一个操作数，然后经过计算再把计算结果压入栈中，最后取栈顶符号作为返回值。  
这道题比较困难的地方在于如何将字符串转化为整数，查阅资料之后发现了`stoi()` 这个api，看了题解之后还发现了`atoi(token.c_str())` 的写法。一开始我还异想天开地以为只需要考虑0~9这十个数字然后写了一个字符串对应字符的哈希表，没有想到再压入栈中的时候整型转化为字符串又出问题了，后来本来想再写一个整型对应字符串的哈希表，但是终于意识到所有的整数不止那么一点随后开始寻找api。  
思路方面还是出了一点问题，寻找api结束之后我以为可以一遍过，但是发现第一个测试用例就过不了，原因是我把栈顶数当成第一个操作数，弹出一个数之后的第二个栈顶数当作第二个操作数了。还有一个问题就是溢出，测试用例里面有一个例子在做乘法的时候溢出了，然后我又一次改成了long型数据，下次注意。  
代码：  
```cpp
class Solution {
private:
public:
    int evalRPN(vector<string>& tokens) {
        vector<int> string;
        int oprand1,oprand2;
        long result;

        for (auto s : tokens) {
            if (s == "+" || s == "-" || s == "*" || s == "/") {
                oprand1 = string.back();
                string.pop_back();
                oprand2 = string.back();
                string.pop_back();
                if (s == "+") result = oprand2 + oprand1;
                else if (s == "-") result = oprand2 - oprand1;
                else if (s == "*") result = (long)oprand2 * oprand1;
                else result = oprand2 / oprand1;
                string.push_back(result);
            } else {
                string.push_back(stoi(s));
            }
        }
        if (string.size() == 1) result = string[0];
        return result;
    }
};
```