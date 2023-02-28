题目描述：  
![image](/basical/IQ/image/image18.png)  
解决过程：  
没想出来，官解的解法是数学，下次这种题可以尝试一下数学的思路  
代码：  
```cpp
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string ans;
        while (columnNumber) {
            --columnNumber;
            ans += columnNumber % 26 + 'A';
            columnNumber /= 26;
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string ans;
        while (columnNumber > 0) {
            int a0 = (columnNumber - 1) % 26 + 1;
            ans += a0 - 1 + 'A';
            columnNumber = (columnNumber - a0) / 26;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```