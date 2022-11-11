题目描述：  
![image](/basical/string/code/reversestrII.md)    
解题过程：  
这个题目跟反转字符串基本是一模一样的，要解决这个题目只需要去关注在对应范围内把字符串给反转了就好了。我自己写了一个反转字符串部分字符的函数，不知道c++还有这样的API，也不知道c++的字符串也可以使用begin()进行迭代器的使用。在写这个函数的时候，参数里面使用了一个地址值为了把s的值在函数里进行改变，但是在调用函数的时候居然不需要使用取地址符？  
代码：  
```cpp
class Solution {
public:
    string reverseStrI(string &s,int start,int end) {
        if (start >= end) return s;
        while (start < end) {
            swap(s[start++],s[end--]);
        }
        return s;
    }

    string reverseStr(string s, int k) {
        int length = (int)s.size();
        if (length < k) return reverseStrI(s,0,length - 1);
        else if (length >= k && length < 2 * k) return reverseStrI(s,0,k - 1);
        int count = (int)length / (2 * k);
        int start = 0,end = k - 1;
        int mod = length % (2 * k);
        while (count--) {
            reverseStrI(s,start,end);
            start += 2 * k;
            end += 2 * k;
        }
        if(mod < k && mod > 0) reverseStrI(s,start,length - 1);
        else if (mod >= k && mod < 2 * k) reverseStrI(s,start,end);
        return s;
    }
};
```