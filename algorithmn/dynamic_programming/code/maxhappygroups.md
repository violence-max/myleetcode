题目描述：  
![image](/algorithmn/dynamic_programming/image/image44.png)  
解题过程：  
这题也太难了吧，状态压缩动态规划，还有leetcode友写出了随机化算法，还有动态规划的优化，真的吐了。  
最关键的点应该在于：一组顾客能否开心取决于之前的顾客的总数是否为batchsize的倍数。题解都是基于这个判断去写的 ，我自己是没有想出来了。基于这个判断，所有可以整除batchsize的每组顾客都可以开心，然而这还不够，在优化的思路上可以直接两两配对：1，batchsize-1，……，这样的一组至少有一波顾客是可以快乐的，基于这个优化思路，最后需要讨论的顾客组数不超过batchsize / 2  
主要来说一说官方题解吧，也会把灵神的优化思路代码和其关键部分放上来  
官解里是用函数的思想去进行动态规划，从一开始，使用位运算对batchsize个状态进行压缩，然后对最后一组顾客进行讨论，如果总数total减去最后一组顾客的个数可以整除n说明在最后一组顾客之前顾客数目是n的倍数，因此最后一组顾客可以开心。基于这个，对最后一组顾客进行枚举，并且结合记忆化搜索进行解题。其中的位运算这些细节还是看代码吧，写下来比较久  
灵神的优化思路的关键是加入了一个left，left在mask的高20位往左，left的语意是当前剩余的没有出售的甜甜圈的个数，因此当left == 0时被枚举到的那组顾客一定是可以快乐的。每次有有关left的更新和mask的重写也很重要，细节见代码  
代码：（状态压缩动态规划→优化）  
```cpp
class Solution {
private:
    static constexpr int kwidth = 5;
    static constexpr int kwidthmask = (1 << kwidth) - 1;
public:
    int maxHappyGroups(int batchSize, vector<int>& groups) {
        vector<int> cnt (batchSize);
        for (int &n : groups) {
            ++cnt[n % batchSize];
        }
        long long start = 0;
        for (int i = batchSize - 1; i >= 1; i--) {
            start = (start << kwidth) | cnt[i];
        }
        unordered_map<long long, int> memo;
        function<int(long long)> dfs = [&](long long mask) -> int {
            if (mask == 0) {
                return 0;
            } else if (memo.count(mask) > 0) {
                return memo[mask];
            }
            int total = 0;
            for (int i = 1; i < batchSize; i++) {
                total += i * ((mask >> ((i - 1) * kwidth)) & kwidthmask);
            }
            int best = 0;
            for (int i = 1; i < batchSize; i++) {
                int amount = (mask >> ((i - 1) * kwidth)) & kwidthmask;
                if (amount > 0) {
                    int result = dfs(mask - (1LL << ((i - 1) * kwidth)));
                    if ((total - i) % batchSize == 0) {
                        ++result;
                    }
                    best = max(best,result);
                }
            }
            return memo[mask] = best;
        };
        return dfs(start) + cnt[0];
    }
};
```
```cpp
class Solution {
    int m;
    vector<int> val;
    unordered_map<int, int> cache;

    int dfs(int mask) {
        if (cache.count(mask)) return cache[mask];
        int res = 0, left = mask >> 20, msk = mask & ((1 << 20) - 1);
        for (int i = 0, j = 0; i < val.size(); ++i, j += 5) // 枚举顾客
            if (msk >> j & 31) // cnt[val[i]] > 0
                res = max(res, (left == 0) + dfs((left + val[i]) % m << 20 | msk - (1 << j)));
        return cache[mask] = res;
    }

public:
    int maxHappyGroups(int batchSize, vector<int> &groups) {
        m = batchSize;
        int ans = 0;
        vector<int> cnt(m);
        for (int x: groups) {
            x %= m;
            if (x == 0) ++ans; // 直接排在最前面
            else if (cnt[m - x]) {
                --cnt[m - x]; // 配对
                ++ans;
            } else ++cnt[x];
        }

        int mask = 0;
        for (int x = 1; x < m; ++x)
            if (cnt[x]) {
                val.push_back(x);
                mask = mask << 5 | cnt[x];
            }
        // 上面加入 val 的顺序和拼接 mask 的顺序是相反的，reverse 后就对上了
        reverse(val.begin(), val.end());

        return ans + dfs(mask);
    }
};
```