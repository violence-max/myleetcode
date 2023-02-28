题目描述：  
![image](/basical/string/image/image32.png)  
解题过程：  
竟然还出错了一次。我一开始的想法是三个三个地检查，如果遇到X就加1。后来发现这种做法不是最优的，测试用例”OXOX”说明了一切。然后我就改思路了：跳过所有的’O’，遇到’X’则答案加1，然后越过三个字符。  
代码：  
```cpp
class Solution {
public:
    int minimumMoves(string s) {
        int ls = (int)s.size();
        int cnt = 0,i = 0;
        while (i < ls) {
            while (i < ls && s[i] != 'X')
                i++;
            if (i == ls) break;
            cnt++;
            i += 3;
        }
        return cnt;
    }
};
```