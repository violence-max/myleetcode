题目描述：  
![image](/basical/IQ/image/image35.png)  
解题过程：  
没做出来  
- 除法求余：
1. n对-2进行求余，可能的结果有-1，0，1，由于所需的字符串的每一位都不能有负数，因此需要对余数进行取绝对值的操作
2. 如果余数小于0，即为-1，商需要加1  
代码：  
```cpp
class Solution {
public:
    string baseNeg2(int n) {
        string ans;
        while(n)
        {
            int remain = n % (-2);
            ans += '0' + abs(remain);
            cout << remain << ' ' << n << endl;
            n = remain < 0 ? n / (-2) + 1 : n / (-2);
        }
        reverse(ans.begin(),ans.end());
        return ans.empty() ? "0" : ans;
    }
};
```