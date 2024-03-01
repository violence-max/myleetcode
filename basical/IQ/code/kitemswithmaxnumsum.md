题目描述：

![image](/basical/IQ/image/image50.png)

解决过程：

3秒钟

代码：

```cpp
class Solution {
public:
    int kItemsWithMaximumSum(int numOnes, int numZeros, int numNegOnes, int k) {
        return numOnes >= k ? k : numZeros >= k - numOnes ? numOnes : numOnes - (k - numOnes - numZeros);
    }
};
```