题目描述：

![image](/basical/IQ/image/image61.png)

解决过程：
智力题没有做出来。。。

代码：

```cpp
class Solution {
public:
    int maxArea(int h, int w, vector<int>& hs, vector<int>& vs) {
        sort(hs.begin(), hs.end());
        sort(vs.begin(), vs.end());
        auto calmax = [](const vector<int>& arr, int border) -> int {
            int res = 0, pre = 0;
            for (int i : arr) {
                res = max(res, i - pre);
                pre = i;
            }
            return max(res, border - pre);
        };
        int mod = 1e9+7;
        return (long long)calmax(hs,h) * calmax(vs,w) % mod;
    }
};
```