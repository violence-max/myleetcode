题目描述：  
![image](/basical/IQ/image/image4.png) 
解题过程：  
md，这sb题目，两数相除，恶心死你叔叔了，我一开始的思路是记录答案的正负号然后取正数进行计算，来了个被除数为INT_MIN，这样子真的是高勾巴，因为取正数会溢出，试了好多次才发现的，然后经过我周密地计算我做了一种取正最大值来计算的方法，结果，超时了，恶心  
代码：  
```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        bool flag = ((dividend < 0 && divisor < 0) || (dividend > 0 && divisor >0)) ? true : false;
        dividend = (dividend >= 0) ? dividend : (dividend == INT_MIN) ? INT_MAX : -dividend;
        divisor = (divisor >= 0) ? divisor : (divisor == INT_MIN) ? -long(divisor) : -divisor;
        int count = 0;
        bool MAX = false;
        if (dividend == INT_MAX) {
            MAX = true;
        }
        while (dividend - divisor >= 0) {
            dividend -= divisor;
            count++;
        }
        if (MAX) {
            count = (dividend - divisor + 1 == 0) ? ((count == INT_MAX) ? count : count + 1) : count;
        }
        return (flag) ? count : -count;
    }
};
```  
快速加+二分法：  
这个方法是寂寞的，利用了快速加和二分法。  
1. 快速加：但加数大于0时开始进行循环，如果加数是奇数则结果先加上当前被加数，如果加数不唯一则被加数翻倍（自身相加），加数右移一位。但是在这个题里面，配合二分查找来做这个题，需要确保每一步的计算结果都不会超出被除数，所以返回值是bool类型的 
2. 二分法：这里的二分法相当角度刁钻，因为可能发生越界的问题，所以一开始处理了特殊的情况，然后用**负数**进行处理。用负数进行处理就真的是强无敌了，我一直的想法就是用正数进行处理，但是一想到最小的负数我就头大，不知道应该怎么操作。 
总的来讲，这个方法是无敌的 
```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        if (dividend == INT_MIN) {
            if (divisor == 1) {
                return INT_MIN;
            }
            if (divisor == -1) {
                return INT_MAX;
            }
        }
        if (divisor == INT_MIN) {
            return dividend == INT_MIN ? 1 : 0;
        }
        if (dividend == 0) {
            return 0;
        }
        bool rev = false;
        if (dividend > 0) {
            dividend = -dividend;
            rev = !rev;
        }
        if (divisor > 0) {
            divisor = -divisor;
            rev = !rev;
        }
        auto quickAdd = [](int y, int z, int x) {
            int result = 0, add = y;
            while (z) {
                if (z & 1) {
                    if (result < x - add) {
                        return false;
                    }
                    result += add;
                }
                if (z != 1) {
                    if (add < x - add) {
                        return false;
                    }
                    add += add;
                }
                z >>= 1;
            }
            return true;
        };
        
        int left = 1, right = INT_MAX, ans = 0;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            bool check = quickAdd(divisor, mid, dividend);
            if (check) {
                ans = mid;
                if (mid == INT_MAX) {
                    break;
                }
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }

        return rev ? -ans : ans;
    }
};
```