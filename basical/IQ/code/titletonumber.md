题目描述：  
![image](/basical/IQ/image/image20.png)  
解决过程：  
30秒ac  
代码：  
```cpp
class Solution {
public:
    int titleToNumber(string columnTitle) {
        int ans = 0, _pow = 0;
        for (int i = columnTitle.size() - 1; i >= 0; i--) {
            ans += (columnTitle[i] - 'A' + 1) * pow(26,_pow++);
        }
        return ans;
    }
};
```