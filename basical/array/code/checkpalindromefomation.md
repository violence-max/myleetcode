题目描述：  
![image](/basical/array/image/image63.png)  
解题过程：  
没做出来  
- 双指针：
1. 假设分割点为k，前缀字符串的长度为k，后缀字符串的长度为n-k
2. 使用双指针，以字符串a或者b分割后得到的前缀字符串为目标回文字符串的前部分，则必须满足a和b的前部分以及后部分倒序相等
3. 对于不相等的中间部分，可以选择是从左侧分割或者右侧分割，因此可以直接检测字符串a和b在这中间部分是否满足为回文字符串，如果满足则可以直接构成回文字符串  
代码：  
```cpp
class Solution {
public:
    bool check(const string& str, int left , int right) {
        while (left < right) {
            if (str[left++] != str[right--]) {
                return false;
            }
        }
        return true;
    }

    bool checkV2(const string& a, const string& b) {
        int l = a.size();
        int left = 0, right = l - 1;
        while (left < right && a[left] == b[right]) {
            left++;
            right--;
        }
        if (left >= right) {
            return true;
        }
        return check(a,left,right) || check(b,left,right);
    }

    bool checkPalindromeFormation(string a, string b) {
        return checkV2(a,b) || checkV2(b,a);
    }
};
```