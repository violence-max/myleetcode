题目描述：：  
![image](/basical/string/image/image53.png)  
解决过程：  
30秒ac  
代码：  
```cpp
class Solution {
public:
    int countAsterisks(string s) {
        int cnt = 0, ls = s.length(), i = 0;
        int result = 0;
        while (i < ls) {
            if (s[i] == '*' && !(cnt & 1)) {
                result++;
            }
            if (s[i] == '|') cnt++;
            i++;
        }
        return result;
    }
};
```