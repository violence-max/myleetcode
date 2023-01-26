题目描述：  
![image](/basical/string/image/image46.png)  
解决过程：  
十分钟解决，弱智一批  
代码：（我的→官解）  
```cpp
class Solution {
public:
    string getSmallestString(int n, int k) {
        if ((k + 25) / 26 > n) return "";
        string ans (n,'a');
        int sub = k - n * 1;
        int end = ans.length() - 1;
        while (end >= 0 && sub > 0) {
            int n = sub > 25 ? 25 : sub;
            sub = sub > 25 ? sub - 25 : 0;
            ans[end--] = 'a' + n;
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    string getSmallestString(int n, int k) {
        string ans;
        for (int i = 1; i <= n; i++) {
            int lower = max(1, k - (n - i) * 26);
            k -= lower;
            ans.push_back('a' + lower - 1);
        }
        return ans;
    }
};
```