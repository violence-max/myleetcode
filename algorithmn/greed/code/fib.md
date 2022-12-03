题目描述：  
![image](/algorithmn/greed/image/image18.png)  
解题过程：  
第一反应是递归，然后觉得太慢了，就直接一个dp  
居然还有快速幂矩阵的写法，强无敌  
代码：  
```cpp
class Solution {
public:
    int fib(int n) {
        if (n == 0) return 0;
        else if (n == 1) return 1;
        else {
            int first = 0,second = 1;
            int ans;
            for (int i = 2; i <= n; i++) {
                ans = first + second;
                first = second;
                second = ans;
            }
            return ans;
        }
    }
};
```