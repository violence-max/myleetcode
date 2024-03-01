题目描述：

![image](/basicaldatastructure/stackandquere/image/image25.png)

解决过程：
第349场周赛t4，没做出来

- 单调栈+二分
- 线段树（TODO）

代码：（单调栈+二分）

```cpp
class Solution {
public:
    vector<int> maximumSumQueries(vector<int>& nums1, vector<int>& nums2, vector<vector<int>>& queries) {
        int n = nums1.size(), m = queries.size();
        vector<int> indecies (n);
        vector<int> indecies2 (m);
        iota(indecies.begin(), indecies.end(), 0);
        iota(indecies2.begin(), indecies2.end(), 0);
        sort(indecies.begin(), indecies.end(), [&](int a, int b){
            // 索引排序，将nums1从大到小排序
            return nums1[a] > nums1[b];
        });
        sort(indecies2.begin(), indecies2.end(), [&](int c, int d){
            // 索引排序，queries按照x从大到小排序
            return queries[c][0] > queries[d][0];
        });

        // 单调栈，数据为[y, x + y]，y递增，x + y递减
        vector<vector<int>> stk;
        vector<int> ans (m, -1);
        int ptr = 0; // 索引数组的下标

        function<int(int)> bsearch = [&](int target) -> int { // 二分查找
            int length = stk.size();
            int left = 0, right = length - 1;
            int ans = -1;
            while (left <= right) {
                int mid = (left + right) >> 1;
                if (stk[mid][0] >= target) {
                    ans = mid;
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            return ans;
        };

        for (int i = 0; i < m; ++i) {
            int x = queries[indecies2[i]][0], y = queries[indecies2[i]][1];
            int j = -1; // 最终要被赋值给ans[i]的答案
            while (ptr < n && nums1[indecies[ptr]] >= x) {
                // 对于nums1中所有大于等于x的项进行处理
                // 由于nums1是降序，queries也按照x降序查询，因此处理过后在stk中保存的项可以直接用于之后的查询

                int ay = nums2[indecies[ptr]], sum = nums1[indecies[ptr]] + ay;
                while (!stk.empty() && stk.back()[1] <= sum) {
                    // 对于单调栈stk中x + y需要递减的解释：
                    // 如果当前的和，即x + y大于等于栈顶的和，假设栈顶的元素为[y1, sum]，则说明y1 <= y
                    // 若y1 == y，因为x + y >= sum，所以y一定是比y1更好的查询
                    // 若y1 < y，则对于大于等于y1的查询yi，y1是无能为力的。而小于y1的查询x + y >= sum
                    // y仍然是比y1更好的查询，因此无论如何[y1,sum]都不会被当作是答案，因此直接pop()

                    stk.pop_back();
                }
                if (stk.empty() || stk.back()[0] < ay) {
                    // 对于单调栈stk中y需要递增的解释：
                    // 假设栈顶元素为[y1,sum]，若y1 == y，又因为sum > x + y，所以y1是比y更好的查询
                    // 若y1 > y，对于比y大的查询，y无能为力，对于比y小的查询，y1结果更好
                    // 因此必须是y1 < y的情况[y, x + y]这组数据才有意义

                    stk.push_back({ay, sum});
                }
                ptr++;
            }

            // 因为在单调栈中y是递增的而总和sum是递减的，因此对于yi的查询，只需要找到第一个大于等于yi的y即可
            j = bsearch(y);
            if (j != -1) {
                // stk中存在大于等于y的yi

                ans[indecies2[i]] = stk[j][1];
            }
        }

        return ans;
    }
};
```