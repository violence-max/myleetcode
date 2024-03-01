题目描述：

![image](/basical/array/image/image88.png)

解决过程：
第352场周赛t4，没做出来，思维没有跟上

- 常规思维
- 贡献法

代码：（O(n^2)→贡献法）

```cpp
class Solution {
public:
    int sumImbalanceNumbers(vector<int>& nums) {
        int n = nums.size();
        int sum = 0;
        for (int i = 0; i < n; ++i) {
            unordered_set<int> visit = {nums[i]};
            int temp = 0;
            for (int j = i + 1; j < n; ++j) {
                temp += visit.count(nums[j]) ? 0 : 1 - visit.count(nums[j] - 1) - visit.count(nums[j] + 1);
                // cout << i << ' ' << j << ' ' << temp << endl;
                sum += temp;
                visit.insert(nums[j]);
            }
            // cout << i << ' ' << temp << endl;
        }
        return sum;
    }
};
```

```cpp
class Solution {
public:
    int sumImbalanceNumbers(vector<int>& nums) {
        int n = nums.size(), right[n], idx[n + 1];
        // cout << n << endl;
        fill(idx, idx + n + 1, n);
        // for (int i = 0; i <= n; ++i) {
        //     cout << idx[i] << ' ';
        // }
        for (int i = n - 1; i >= 0; --i) {
            int x = nums[i];
            // cout << x << ' ' << idx[x] << ' ' << idx[x - 1] << endl;
						// right[i]表示x和x-1索引的最小值，这里出现x是因为要计算x的贡献值时，只允许左边
						// 有重复的x，右边如果出现重复的数字则会进行多余的子数组
            right[i] = min(idx[x], idx[x - 1]); 
            idx[x] = i;
        }
        int ans = 0;
        memset(idx, -1, sizeof idx);
        for (int i = 0; i < n; ++i) {
            int x = nums[i];
            cout << i << ' ' << idx[x - 1] << ' ' << right[i] << endl;
            ans += (i - idx[x - 1]) * (right[i] - i);
            idx[x] = i;
        }
				// 每个子数组排序后都会有一个最小值，在上面的计算过程中，
				// 由于每次计算都避开了x - 1，因此在包含x - 1的部分x一定不是最小值
				// 所以在(idx[x - 1], right[i])区间里x是可以作为最小值的
				// x作为最小值枚举出来的子数组数目恰好为nums的子数组数目
        return ans - n * (n + 1) / 2;
    }
};
```