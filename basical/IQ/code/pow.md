题目描述：  
![image](/basical/IQ/image/image7.png)  
解题过程：  
快速幂算法，太熟悉了没办法。但是重新写的时候我还是忘记了一点东西：初始化res值，幂进行二分。还有，这道题是要包含取幂数为负的情况的，所以还要加上起初的判断条件，如果幂是负的结果返回的时候要用1来除。  
以及最好用long型对底数相乘的时候进行修饰，因为在得到答案之前底数还要再取 一次平方。  
代码：  
```cpp
class Solution {
public:
    double myPow(double x, int n) {
        bool flag = false;
        if (n < 0) flag = true;
        double res = 1;
        while (n) {
            if (n & 1) {
                res *= x;
            }
            x = x * x;
            n /= 2;
        }
        return (!flag) ? res : 1 / res;
    }
};
```