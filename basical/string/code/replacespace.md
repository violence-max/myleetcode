题目描述：  
![image](/basical/string/image/image7.png)   
解题过程：  
这个是之前做过的题目，我想的比较简单，就是使用O(n)的空间复杂度，然后遇到空格就添加”%20”，否则添加原字符，但是太慢了。。。。  
代码：  
```cpp
class Solution {
public:
    string replaceSpace(string s) {
        string result;
        int length = (int)s.size();
        int i = 0,count = 0;
        while (i < length) {
            if (s[i] == ' ') {
                result.push_back('%');
                result.push_back('2');
                result.push_back('0');
            } else {
                result.push_back(s[i]);
            }
            ++i;
        }
        return result;
    }
};
```