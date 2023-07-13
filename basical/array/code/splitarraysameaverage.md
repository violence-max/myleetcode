题目描述：

![image](/basical/array/image/image92.png)

解决过程：

没做出来

- 折半搜索

代码：（折半搜索→动态规划（0-1背包））

```cpp
class Solution {
public:
    bool splitArraySameAverage(vector<int>& nums) {
        // 假设可以分割k个元素到数组A，n-k个元素到数组B，且A和B均值相等
        // 则sum(A) / k == sum(B) / (n - k)，即sum(A) * n ==  (sum(A) + sum(B)) * k
        // 也即sum(A) / k == sum(nums) / n，因此分割出的两个数组的均值和原数组的均值相等
        int n = nums.size();
        int sum = accumulate(nums.begin(), nums.end(), 0);
        // 剪枝操作：若可以进行这样的分割，那么sum(A) == sum(nums) * k / n，则sum(nums) * k mod(n) == 0
        // 又由于分割出的两个数组均需满足该等式，则必定有一个数组的个数小于等于n / 2，因此可以对k进行枚举进行提前判断
        bool find = false;
        for (int k = 1; k <= n / 2; ++k) if ((sum * k) % n == 0) {find = true;break;}
        if (!find) return false;
        for (int& x : nums) {
            // 对于值的改变并不影响最终结果，因为nums的平均值不变
            // nums中若每个值减去平均值，那么nums的总和为0，此时若能找出nums
            // 的一个真子集且该真子集的和也为0，那么分割的这两部分均值相等
            // 为了避免均值是浮点数带来的影响，这里统一乘以n
            x = n * x - sum;
        }

        // 枚举真子集的总和时间复杂度过高，可以采取分半枚举的方式：先枚举前半部分的总和，同时
        // 使用哈希表记录，再枚举后半部分的总和，若前后两部分总和相加为0，那么也可以找到合适的分割方式

        unordered_set<int> s;
        int m = n / 2;
        for (int i = 1; i < (1 << m); ++i) {
            int tot = 0;
            for (int j = 0; j < m; ++j) {
                if (i & (1 << j)) {
                    tot += nums[j];
                }
            }
            if (tot == 0) return true;
            s.insert(tot);
        }

        int rsum = accumulate(nums.begin() + m, nums.end(), 0);
        for (int i = 1; i < (1 << (n - m)); ++i) {
            int tot = 0;
            for (int j = m; j < n; ++j) {
                if (i & (1 << (j - m))) {
                    tot += nums[j];
                }
            }
            if (tot == 0 || (rsum != tot && s.count(-tot))) return true;
        }
        return false;
    }
};
```

```cpp
class Solution {
public:
    bool splitArraySameAverage(vector<int>& nums) {
        int n = nums.size(), m = n / 2;
        int sum = accumulate(nums.begin(), nums.end(), 0);
        bool isPossible = false;
        for (int i = 1; i <= m; i++) {
            if (sum * i % n == 0) {
                isPossible = true;
                break;
            }
        }  
        if (!isPossible) {
            return false;
        }
        vector<unordered_set<int>> dp(m + 1);
        dp[0].insert(0);
        for (int num: nums) {
            for (int i = m; i >= 1; i--) {
                for (int x: dp[i - 1]) {
                    int curr = x + num;
                    if (curr * n == sum * i) {
                        return true;
                    }
                    dp[i].emplace(curr);
                } 
            }
        }
        return false;
    }
};
```