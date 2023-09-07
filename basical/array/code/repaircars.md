题目描述：

![image](/basical/array/image/image104.png)

解决过程：

没做出来

- 枚举答案加二分判断

代码：

```cpp
class Solution {
public:
    long long repairCars(vector<int>& ranks, int cars) {
        function<bool(long long t)> check = [&](long long t) -> bool {
            long long cnt = 0;
            for (auto x : ranks) {
                cnt += sqrt(t / x);
            }
            return cnt >= cars;
        };
        long long left = 0, right = (long long)ranks[0] * cars * cars;
        while (left < right) {
            long long mid = (left + right) >> 1;
            if (check(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```