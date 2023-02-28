题目描述：  
![image](/algorithmn/dynamic_programming/image/image50.png)  
解决过程：  
没有想出O(n)的解法  
- Brian Kernighan算法：  
    计算整数x的二进制里有多少个1的算法，但是是通过x每次和x-1相与来进行计算，因为每次相与意味着x的最后一个1变成0  
- 动态规划：最高有效位
    1. 对于整数x，如果可以求出其二进制数中最高位的1的位置，即得到其最高有效位，也即得到数y，数y为2的整数次幂，那么容易知道bit[x] = bit[x-y] + 1。这个加上的1即为最高有效位
    2. 0的最高有效位为0
    3. 判断一个数是不是2的整数次幂：对于数y，如果y & (y-1) == 0，说明y的二进制数中仅存在一个1，即y为2的整数次幂
    4. 整个算法流程：从1开始往后遍历，hightbit按照第三点维护最高有效位，根据第一点计算遍历到的数的二进制数中1的个数
- 动态规划：最低有效位  
    对于数x，将其右移一位得到数y，如果数x为偶数，则bit[x] = bit[y]，否则bit[x] = bit[y] + 1   
- 动态规划：最低设置位  
    对于数x，令y = x & (x-1)，则bit[x] = bit[y] + 1  
代码：（Brian Kernighan算法→动态规划（最高有效位））  
```cpp
class Solution {
public:
    int popcount(int x) {
        int ones = 0;
        while (x > 0) {
            x &= (x - 1);
            ones++;
        }
        return ones;
    }

    vector<int> countBits(int n) {
        vector<int> bit (n + 1);
        for (int i = 0; i <= n; i++) {
            bit[i] = popcount(i);
        }
        return bit;
    }
};
```  
```cpp
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> bits(n + 1);
        int highBit = 0;
        for (int i = 1; i <= n; i++) {
            if ((i & (i - 1)) == 0) {
                highBit = i;
            }
            bits[i] = bits[i - highBit] + 1;
        }
        return bits;
    }
};
```  
```cpp
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> bits(n + 1);
        for (int i = 1; i <= n; i++) {
            bits[i] = bits[i >> 1] + (i & 1);
        }
        return bits;
    }
};
```

```cpp
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> bits(n + 1);
        for (int i = 1; i <= n; i++) {
            bits[i] = bits[i & (i - 1)] + 1;
        }
        return bits;
    }
};
```