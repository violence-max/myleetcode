题目描述：

![image](/basical/array/image/image80.png)

解决过程：

2分钟ac

代码：（暴力→排序→哈希表）

```cpp
class Solution {
public:
    int unequalTriplets(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n - 2; ++i) {
            int x = nums[i];
            for (int j = i + 1; j < n - 1; ++j) {
                int y = nums[j];
                for (int k = j + 1; k < n; ++k) {
                    int z = nums[k];
                    ans += (x != y) && (x != z) && (y != z);
                    // cout << x << ' ' << y << ' ' << z << endl;
                }
            }
        }
        return ans;
    }
};
```

```cpp
class Solution {
public:
    int unequalTriplets(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int res = 0, n = nums.size();
        for (int i = 0, j = 0; i < n; i = j) {
            while (j < n && nums[j] == nums[i]) {
                j++;
            }
            res += i * (j - i) * (n - j);
        }
        return res;
    }
};
```

```cpp
class Solution {
public:
    int unequalTriplets(vector<int>& nums) {
        unordered_map<int, int> count;
        for (auto x : nums) {
            count[x]++;
        }
        int res = 0, n = nums.size(), t = 0;
        for (auto [_, v] : count) {
            res += t * v * (n - t - v);
            t += v;
        }
        return res;
    }
};
```