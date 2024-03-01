题目描述：

![image](/basical/array/image/image102.png)

解决过程：

第110场双周赛t3，ac

代码：

```cpp
class Solution {
public:
    int id[100020];
    int minimumSeconds(vector<int>& nums) {
        unordered_map<int, int> pos;
        int n = nums.size();
        for (int i = n - 1; i >= 0; --i) {
            if (pos.count(nums[i])) {
                id[i] = pos[nums[i]];
            } else {
                id[i] = -1;
            }
            pos[nums[i]] = i;
            // cout << i << ' ' << id[i] << endl;
        }
        int ret = INT_MAX;
        for (int i = 0; i < n; ++i) {
            int in = 0;
            int j = i;
            while (id[j] != -1) {
                in = max(in, (id[j] - j) / 2);
                j = id[j];
            }
            in = max(in, (n - (j - i + 1) + 1) / 2);
            ret = min(ret, in);
        }
        return ret;
    }
};
```