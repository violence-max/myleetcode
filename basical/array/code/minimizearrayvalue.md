题目描述：

![image](/basical/array/image/image106.png)

解决过程：
没做出来

- 二分答案
- 贪心：取n个数的平均值

代码：

```cpp
class Solution {
public:
    int minimizeArrayValue(vector<int>& nums) {
        int n = nums.size();
        int mxx = *max_element(nums.begin(), nums.end());
        int mnn = 0;
        if (mxx == 0) return mxx;
        cout << mxx << endl;
        while (mnn < mxx) {
            int mid = (mnn + mxx) >> 1;
            long long extra = 0;
            for (int i = n - 1; i > 0; --i) {
                extra = max((long long)nums[i] + extra - mid, (long long)0);
            }
            if (nums[0] + extra <= mid) {
                mxx = mid;
            } else {
                mnn = mid + 1;
            }
        }
        return mxx;
    }
};
```

```cpp
class Solution {
public:
    int minimizeArrayValue(vector<int>& nums) {
        int n = nums.size();
        long long sum = 0;
        int ans = nums[0];
        for (int i = 0; i < n; ++i) {
            sum += (long long)nums[i];
            ans = max((long long)ans, (sum + i) / (i + 1));
        }
        return ans;
    }
};
```