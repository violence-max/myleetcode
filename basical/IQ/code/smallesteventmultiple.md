题目描述：  
![image](/basical/IQ/image/image38.png)  
解决过程：  
3s解决  
代码：  
```cpp
class Solution {
public:
    int smallestEvenMultiple(int n) {
        return (n & 1) ? n * 2 : n;
    }
};
```