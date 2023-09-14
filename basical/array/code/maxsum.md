题目描述：

![image](/basical/array/image/image107.png)

解决过程：

没做出来

- 抽象成有向图结合归并排序的做法

代码：

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;
    int maxSum(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        long long b1 = 0, b2 = 0;
        int i = 0, j = 0;
        while (i < m || j < n) {
            if (i < m && j < n) {
                if (nums1[i] < nums2[j]) { // 小的那方追赶大的那方来得到相同的元素
                    b1 += nums1[i++];
                } else if (nums1[i] > nums2[j]) {
                    b2 += nums2[j++];
                } else {
                    long long best = max(b1, b2) + nums1[i]; // 从最大的那方转移过来
                    b1 = b2 = best;
                    i++;
                    j++;
                }
            } else if (i < m) {
                b1 += nums1[i++];
            } else if (j < n) {
                b2 += nums2[j++];
            }
        }
        return max(b1, b2) % mod; // 只有最后取答案的时候才取模，否则之前的最大值可能会被模掉
    }
};
```