题目描述：  
![image](/basical/string/image/image34.png)  
解决过程：  
傻逼题目，提交居然出错了五六次，不过今天是改头换面第一天。  
算法流程：  
1. 用ls记录字符串长度，left为左指针，right为右指针，分别指向字符串的头部和尾部，用ans记录答案，起初赋值为ls
2. 开始进行while循环，当左指针越过右指针时退出循环
3. while循环里做一个ifelse条件语句判断：如果左指针指向的值与右指针指向的值不相等，则退出循环；否则左指针加1，右指针减1，此时若左右指针相等则返回1（bab这种测试用例），若左指针越过了右指针则返回0（aa这种测试用例）；否则左指针一直向右移动至右指针处或者与右指针的值不相等为止，若此时左右指针相等，则返回0（bbbbbbbb这种测试用例），否则右指针一直向左移动直至与原先的值不相等为止，若此时左右指针相等则返回1（bbabb这种测试用例），否则用ans记录左右指针的子串的长度作为答案记录  
这个题最脑瘫的地方在于左右指针越界的把控和答案的记录，题解以一种看似巧妙实际上玄乎的方式得到了答案，因为在得到答案之后有可能左指针已经越过右指针了，但是由于答案是左右指针的子串的长度，最后的加1可以返回正确的值0  
代码：  
```cpp
class Solution {
public:
    int minimumLength(string s) {
        int ls = (int)s.size();
        int left = 0,right = ls - 1;
        int ans = ls;
        while (left < right) {
            if (s[left] != s[right]) {
                break;
            } else {
                ++left; --right;
                if (left > right) return 0;
                if (left == right) return 1;
                while (left < right && s[left] == s[left-1])
                    left++;
                if (left == right) return 0;
                while (s[right] == s[right+1])
                    --right;
                if (left == right) return 1;
                ans = right - left + 1;
            }
        }
        return ans;
    }
};
```  
题解：  
```cpp
class Solution {
public:
    int minimumLength(string s) {
        int ls = (int)s.size();
        int left = 0,right = ls - 1;
        while (left < right && s[left] == s[right]) {
            char c = s[left];
            while (left <= right && s[left] == c)
                left++;
            while (left <= right && s[right] == c)
                right--;
        }
        return right - left + 1;
    }
};
```