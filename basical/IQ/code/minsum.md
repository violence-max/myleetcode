题目描述：

![image](/basical/IQ/image/image62.png)

解决过程：

第369场周赛t2，没做出来

其实是一个脑筋急转弯的题目，先算出每个数组的最小元素和，再进行大小判断

代码：

```cpp
class Solution {
public:
    long long minSum(vector<int>& nums1, vector<int>& nums2) {
        long long sum1 = 0, sum2 = 0;
        int cnt1 = 0, cnt2 = 0;
        for (int x : nums1) {
            sum1 += x;
            cnt1 += x == 0;
        }
        for (int x : nums2) {
            sum2 += x;
            cnt2 += x == 0;
        }
        if (((sum1 + cnt1) < (sum2 + cnt2) && cnt1 == 0) || ((sum1 + cnt1) > (sum2 + cnt2) && cnt2 == 0)) return -1;
        return max(sum1 + cnt1, sum2 + cnt2);
    }
};
```