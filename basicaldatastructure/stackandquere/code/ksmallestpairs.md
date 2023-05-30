题目描述：

![image](/basicaldatastructure/stackandquere/image/image21.png)

解决过程：

没做出来

- 优先队列
- 二分查找（没看）

代码：（优先队列）

```bash
class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        auto cmp = [&nums1, &nums2](const pair<int,int> &a, const pair<int,int> &b) {
            // 优先队列的小根堆
            return nums1[a.first] + nums2[a.second] > nums1[b.first] + nums2[b.second];  
        };

        int m = nums1.size();
        int n = nums2.size();
        vector<vector<int>> ans;
        priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);
        for (int i = 0; i < min(k,m); i++) {
            // 若(a_i, b_i)是第i小的数对，则第i+1小的数对为(a_{i+1},b_i)或者(a_i,b_{i+1})
            // 如果每次得到了第i小的数对都把待选项加入堆中，则会出现重复，
            // 因此这里提前先把nums1中前k个值和nums2中索引为0的值加入堆中，如果{x,y}为第i小的数对，
            // 则只需要将{x,y+1}加入堆中即可
            // 不需要加{x+1,y}是因为此时堆中一定存在{x+1,<=y}的数对，这个算法的本质思想是：
            // 维护一个只有k个数的堆，每次弹出堆顶元素并且加入一个待比较元素
            pq.emplace(make_pair(i, 0));
        }
        while (k-- > 0 && !pq.empty()) {
            auto [x,y] = pq.top();
            pq.pop();
            ans.push_back({nums1[x], nums2[y]});
            if (y + 1 < n) {
                pq.emplace(x, y + 1);
            }
        }

        return ans;
    }
};
```