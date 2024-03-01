题目描述：

![image](/basical/array/image/image117.png)

解决过程：

太久不练了，思维已经退化到老年人的地步了，需要重新捡起来一些东西了。

这个题目很好地枚举了答案，这种思维值得学习。

代码：

```cpp
class Solution {
public:
    int findKOr(vector<int>& nums, int k) {
        int res = 0;
        for (int i = 0; i < 31; ++i) {
            int cnt = 0;
            for (int x : nums) {
                cnt += ((1 << i) & x) > 0 ? 1 : 0;
            }
            if (cnt >= k) res += (1 << i);
        }
        return res;
    }
};
```