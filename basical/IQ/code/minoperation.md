题目描述：

![image](/basical/IQ/image/image63.png)

解决过程：

第371场周赛t3，没做出来

只需要考虑每个数组的最后一个数字，对于其余的数字，只需要考虑交换或者不交换两种情况

代码：

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums1, vector<int>& nums2) {
        function<int(int,int)> f = [&](int last1, int last2) -> int {
            int res = 0;
            for (int i = 0; i < nums1.size() - 1; ++i) {
                int x = nums1[i], y = nums2[i];
                if (x > last1 || y > last2) {
                    if (y > last1 || x > last2) {
                        return INT_MAX / 2;
                    }
                    res++;
                }
            }
            return res;
        };
        int res1 = f(nums1.back(), nums2.back());
        int res2 = f(nums2.back(), nums1.back());
        return (res1 == INT_MAX / 2 && res2 == INT_MAX / 2) ? -1 : min(res1, 1 + res2);
    }
};
```