题目描述：  
![image](/basical/IQ/image/image1.png)  
解题过程：看到就直接投了，就算看了题解还是不是很清楚，这个题目涉及到一定的数学知识，因为这个快乐数最终的结局要么是回到1要么是死循环，根本不会陷入无穷大里面，而如果要严格证明这个东西我估计要高斯来做了。如果知道这个快乐数的结局我觉得我肯定能做，就是用哈希表就可以了，不存在哈希表且不为1则加入哈希表然后继续往下找，否则退出然后判断结果是否正确就行。  
代码：  
```cpp
class Solution {
public:
    int getNext(int n) {
        int digits,sum = 0;
        while (n) {
            digits = n % 10;
            sum += digits * digits;
            n = (n - digits) / 10;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set <int> set;
        while (n != 1 && !set.count(n)) {
            set.insert(n);
            n = getNext(n);
        }
        return n == 1;
    }
};
```