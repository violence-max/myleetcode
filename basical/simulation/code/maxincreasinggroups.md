题目描述：

![image](/basical/simulation/image/image9.png)

解决过程：

第355场周赛t3，不会

- 构造法：考虑答案的可能区间：[0,n]，n为数组长度。结合二分法检查每个答案的可能性，同时保存答案。检查的方法：将数组排序后倒序，那么得到了一个值从大到小排序的频率数组。对于每个检查的数字mid，我们需要考虑每个频率的最小值，即mid、mid-1、……、1，这是能够得到答案mid的每个位置上频率最小的频率数组。（具体见利口题解，不想码了，主要就是利用a[i]-t[i]的差值来填补之前的空缺，得到一个左下三角形，按行从上往下遍历就是构造方案）

代码：

```cpp
class Solution {
public:
    int maxIncreasingGroups(vector<int>& usageLimits) {
        sort(usageLimits.begin(), usageLimits.end(), greater<int>());
        int left = 0, right = usageLimits.size();
        int ans = -1;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            int gap = 0;
            int tmp = mid;
            for (auto x : usageLimits) {
                gap = min(gap + x - tmp, 0);
                if (tmp > 0) tmp--;
            }
            // cout << left << ' ' << mid << ' ' << right << ' ' << gap << endl;
            if (gap >= 0) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
};
```