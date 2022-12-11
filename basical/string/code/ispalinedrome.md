题目描述：  
![image](/basical/string/image/image15.png)  
解决过程：  
直接用的字符串反转，但是确实乐色  
看了题解，反转一半整数的方法简直绝了，强无敌  
代码：  
```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return false;
        string str = to_string(x),s = to_string(x);
        reverse(str.begin(),str.end());
        if (s == str) return true;
        else return false;
    }
};
```