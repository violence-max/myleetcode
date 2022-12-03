题目描述：  
![image](/algorithmn/dynamic_programming/image/image3.png)  
解题过程：  
额，居然没有一次ac，一开始以为是选前两次的最大值，由前两级台阶+2，或者前一级台阶加1决定。后来恍然大悟应该是前两级台阶*2，再后来发现不需要取最大值，直接相加就好，再后来发现会有重复部分，这其实就是斐波那契数列，无语。  
代码：  
```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) return n;
        int first = 1,second = 2;
        int ans;
        for (int i = 3; i <= n; i++) {
            ans = first + second;
            first = second;
            second = ans;
        }
        return ans;
    }
};
```