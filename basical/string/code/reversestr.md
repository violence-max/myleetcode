题目描述：  
![image](/basical/string/image/image5.png)    
解题过程：  
直接双指针，没有什么好说的  
（python可以直接逆序输出）  
代码：  
```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int left = 0, right = (int)s.size() - 1;
        while (left < right) {
            swap(s[left++],s[right--]);
        }
    }
};
```