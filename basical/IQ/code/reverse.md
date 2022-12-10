题目描述：  
![image](/basical/IQ/image/image2.png)  
解题过程：  
直接疯狂对10求余然后加到结果上  
出现了两个问题：  
1. 尽管是负数，但是采用对正数的操作仍然可以行得通—负数对10求余也是负数
2. 反转时会发生溢出，需要用long型接住判断是否超出INT的范围  
代码：  
```cpp
class Solution {
public:
    int reverse(int x) {
        int count = 0;
        long ans = 0;
        while (x != 0) {
            int mod = x % 10;
            ++count;
            ans = (long)ans * 10 + mod;
            if (ans < INT_MIN || ans > INT_MAX) return 0;
            x /= 10;
        }
        return ans;
    }
};
```