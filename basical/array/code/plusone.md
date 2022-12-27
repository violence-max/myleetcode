题目描述：  
![image](/basical/array/image/image29.png)  
弱智一批，简单加法模拟  
代码：  
```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int m = digits.size();
        int cin = 0;
        for (int i = m - 1; i >= 0; i--) {
            digits[i] += (i == m - 1) ? 1 : cin;
            cin = digits[i] >= 10 ? 1 : 0;
            digits[i] %= 10;
            if (cin == 0) break;
        }
        if (cin == 1)
            digits.insert(digits.begin(),1);
        return digits;
    }
};
```