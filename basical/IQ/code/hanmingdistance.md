题目描述：  
![image](/basical/IQ/image/image28.png)  
解决过程：  
用的0到31位注意比较解决的，看了题解，直接对x ^ y进行求其二进制中1的数量  
代码：  
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int bit = 0, ret = 0;
        while (bit < 32) {
            int _bit = 1 << bit;
            ret += ((x & (_bit)) ^ (y & (_bit))) != 0;
            bit++;
        }
        return ret;
    }
};
```