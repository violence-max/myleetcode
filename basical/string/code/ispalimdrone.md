题目描述：  
![image](/basical/string/image/image45.png)  
解决过程：  
so easy。不过，没有看到连数字字符也要算，被测试用例”0P”给坑了。学到了几个api，isalnum,tolower  
代码：  
```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int ls = s.length();
        int left = 0, right = ls - 1;
        while (left <= right) {
            while (left <= right && !isupper(s[left]) && !islower(s[left]) && !isdigit(s[left]))
                left++;
            while (left <= right && !isupper(s[right]) && !islower(s[right]) && !isdigit(s[right]))
                right--;
            if (left > right) break;
            s[left] = (isupper(s[left])) ? (s[left]+32) : s[left];
            s[right] = (isupper(s[right])) ? (s[right]+32) : s[right];
            if (s[left] != s[right]) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
};
```