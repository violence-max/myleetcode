题目描述：

![image](/algorithmn/dynamic_programming/image/image69.png)

解决过程：

是状态压缩的思路，但是做了40分钟还报错，就参考题解了。对于题解最终获取答案的方式表示应该是有更加优秀的做法的，因为题解最后获取n/k个元素的集合是通过子集遍历实现的。

代码：

```cpp
class Solution {
public:
    int minimumIncompatibility(vector<int>& nums, int k) {
        int n = nums.size();
        unordered_map<int, int> values;
        int c = n / k;
        function<void(int, int , int)> Cnk = [&](int mask, int prev, int cnt) -> void {
            if (cnt == c) {
                // cout << prev << ' ';
                unordered_set<int> se;
                int mxx = INT_MIN, mnn = INT_MAX;
                for (int i = 0; i < n; ++i) {
                    if (prev & (1 << i)) {
                        if (se.count(nums[i])) {
                            break;
                        }
                        se.insert(nums[i]);
                        mxx = max(mxx, nums[i]);
                        mnn = min(mnn, nums[i]);
                    }
                }
                if (se.size() == c) {
                    values[prev] = mxx - mnn;
                }
                return;
            }

            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    Cnk(mask ^ (1 << i), prev | (1 << i), cnt + 1);
                    mask = mask ^ (1 << i);
                }
            }
        };
        int log = 1 << n;
        Cnk(log - 1, 0, 0);
        // cout << "size is : " << values.size() << endl;

        vector<int> dp (1 << n, INT_MAX);
        dp[0] = 0;
        for (int i = 0; i < log; ++i) {
            if (dp[i] == INT_MAX) {
                continue;
            }
            unordered_map<int,int> seen;
            for (int j = 0; j < n; ++j) {
                if (!(i & (1 << j))) {
                    seen[nums[j]] = 1 << j;
                }
            }
            if (seen.size() < c) {
                continue;
            }
            int sub = 0;
            for (const auto& p : seen) {
                sub |= p.second;
            }
            int nxt = sub;
            while (nxt) {
                if (values.count(nxt)) {
                    dp[i | nxt] = min(dp[i | nxt], dp[i] + values[nxt]);
                }
                nxt = (nxt - 1) & sub;
            }
        }
        return dp[log - 1] < INT_MAX ? dp[log - 1] : -1;
    }
};
```