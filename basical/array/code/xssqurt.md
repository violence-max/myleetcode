题目描述：
![image](/basical/array/image/image5.png)
解题过程：  
方法一：袖珍计算器  
使用表达式`exp(0.5 * log(x))` 来代替x的平方根符号，再对结果进行微处理即可，因为浮点数存储得不精确，有可能答案和结果相差非常小，但是结果小于答案，而我们的返回值取的是整数，所以会有返回值比正确值小1的情况  
代码：  
```cpp
class Solution {
public:
    int mySqrt(int x) {
        if (x == 0) return 0;
        int result = exp(0.5 * log(x));
        return ((long long)(result + 1) * (result + 1) <= x) ? result + 1 : result;
    }
};
```
方法二：二分查找  
以0到x为区间进行二分查找，如果数的平方大于x则在该数的右侧进行查找，如果数的平方小于等于x则在该数的左侧进行查找  
细节：  
1. 因为要找的值是正确值的整数部分，所以我们在数的平方小于等于x时记录结果而不是在数的平方大于等于x时记录结果
2. x是一个32位的二进制数，而x之间的任意两个数的乘积都有可能越界，因此要在第一个数之前加一个long long进行强转，不能对结果进行强转，因为结果有可能已经越界了，先返回一个int的值再进行强转是不行的 
代码：  
```cpp
class Solution {
public:
    int mySqrt(int x) {
        int left = 0,right = x;
        int mid,result = -1;
        while (left <= right) {
            mid = (left + right) >> 1;
            if ((long long)mid * mid <= x) {
                result = mid;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return result;
    }
};
```