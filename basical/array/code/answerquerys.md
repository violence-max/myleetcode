题目描述：  
![image](/basical/array/image/image62.png)  
解决过程：  
没做出来  
- 二分查找
1. 需要对数组进行排序预处理
2. 计算前缀和数组f
3. 在f中找到对于查询q最小的i并且满足f[i] > q， 答案则是i - 1  
代码：  
```cpp
class Solution {
public:
    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) {
        int n = nums.size(), m = queries.size();
        sort(nums.begin(), nums.end());
        vector<int> f(n + 1);
        for (int i = 0; i < n; i++) {
            f[i + 1] = f[i] + nums[i];
        }
        vector<int> answer(m);
        for (int i = 0; i < m; i++) {
            answer[i] = upper_bound(f.begin(), f.end(), queries[i]) - f.begin() - 1;
        }
        return answer;
    }
};
```