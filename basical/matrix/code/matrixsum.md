题目描述：

![image](/basical/matrix/image/image18.png)

解决过程：

- 排序
- 优先队列；有序集合

代码：（有序集合）

```cpp
class Solution {
public:
    int matrixSum(vector<vector<int>>& nums) {
        int res = 0;
        int m = nums.size(), n = nums[0].size();
        unordered_map<int, multiset<int>> p;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                p[i].insert(nums[i][j]);
            }
        }
        for (int i = 0; i < n; ++i) {
            int mxx = INT_MIN;
            for (int j = 0; j < m; ++j) {
                int temp = *p[j].rbegin();
                mxx = max(mxx, temp);
                // cout << 
                p[j].erase(prev(p[j].end()));
            }
            // cout << i << ' ' << mxx << endl;
            res += mxx;
        }
        return res;
    }
};
```