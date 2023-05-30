题目描述：

![image](/basical/IQ/image/image39.png)

解决过程：

30s

代码：

```cpp
class Solution {
public:
    int averageValue(vector<int>& nums) {
        int sum = 0 , count = 0;
        for (int i : nums) {
            if (i % 3 == 0 & i % 2 == 0) {
                sum += i;
                count++;
            } 
        }
        if (count == 0) return 0;
        else return sum / count;
    }
};
```