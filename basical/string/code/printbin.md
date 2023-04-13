题目描述：  
![image](/basical/string/image/image59.png)  
解决过程：  
通过模拟三秒钟ac  
看了题解，发现几个两点：  
1. 任何进制的小数乘上进制相当于小数点往右移一位。在这个题目里面，对于从字符串`”0.”` 开始往末位添加字符1或者字符0，就可以有以下逻辑：num *= 2;如果num < 1说明应该添加字符0，否则应该添加1;—num过后如果num == 0就可以得到有效答案
2. 灵神证明了只需要循环6次就可以判断一个小数是否可以被二进制小数表示了，不过这里的证明我没有细看  
代码：  
```cpp
class Solution {
public:
    string printBin(double num) {
        if (num == 0 || num == 1.0) return to_string((int)num);
        int substript = 1;
        string ans = "0.";
        while (num) {
            if (substript > 32) return "ERROR";
            double power = 1.0 / pow(2,substript);
            if (num >= power) {
                num -= power;
                ans += '1';
            } else ans += '0';
            substript++;
        }
        return ans;
    }
};
```