题目描述：

![image](/basical/IQ/image/image58.png)

解决过程：

没有做出来

- 位运算

代码：

```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int xorsum = 0;
        for (auto num : nums) {
            xorsum ^= num;
        }
        int lsb = (xorsum == INT_MIN ? xorsum : xorsum & (-xorsum));
        int type1 = 0, type2 = 0;
        for (auto num : nums) {
            if (num & lsb) {
                type1 ^= num;
            } else {
                type2 ^= num;
            }
        }
        return {type1, type2};
    }
};
```