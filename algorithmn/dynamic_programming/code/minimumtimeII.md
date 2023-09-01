题目描述：

![image](/algorithmn/dynamic_programming/image/image92.png)

解决过程：

第110场双周赛t4，没做出来

代码：

```
class Solution {
public:
    int f[1001];
    pair<int,int> a[1000];
    int minimumTime(vector<int>& nums1, vector<int>& nums2, int x) {
        int n = nums1.size(), s1 = 0, s2 = 0;
        for (int i = 0; i < n; ++i) {
            a[i] = make_pair(nums2[i], nums1[i]);
            s1 += nums1[i];
            s2 += nums2[i];
        }
        sort(a, a + n); // 根据nums2[i]从小到大排序
        memset(f, 0, sizeof f); // f[j]表示在nums1中选取长度为j的子序列，这个子序列将按照顺序依次进行置零操作，总共进行j次置零操作
        for (int i = 0; i < n; ++i) {
            // 考虑下标0~i的子数组中长度为j的子序列
            for (int j = i + 1; j; --j) {
                // 将nums2从小到大排序的原因是：要想使得最后一次置零操作得到的收益最大，即减少的值最大
                // 那么这个对应的值必须是最大的
                // 考虑一个对长度为j的子序列a1,a2,a3,……,aj进行操作，当j秒过去后，
                // 每个位置上的值为：(j-1)*nums2[a1],(j-2)*nums2[a2],(j-3)*nums3[a3],……,0
                // 总共减少的值为nums1[a1]+nums2[a1],nums1[a2]+2*nums2[a2],nums1[a3]+3*nums2[a3],……,nums1[aj]+j*nums2[aj]
                // 同样操作j次的情况下，减少的值当然是越大越好，因此f的状态转移方程显而易见
                f[j] = max(f[j], f[j-1] + a[i].first * j + a[i].second); // 选或者不选a[i]作为子序列的最后一个数字
            }
        }
        for (int t = 0; t <= n; ++t) { // 从小到达枚举答案，根据鸽巢原理，同一个数不会选择重复置零，因为这样相当于增大值
            if (s1 + t * s2 - f[t] <= x) return t;
        }
        return -1;
    }
};
```