题目描述：

![image](/basicaldatastructure/stackandquere/image/image22.png)

解题过程：

没做出来

和[查找和最小的k个数字极度类似](/basicaldatastructure/stackandquere/code/ksmallestpairs.md)

- 使用const重载运算符的时候还得注意是否要加const。。。因为有可能会修改对象的状态

代码：（优先队列）

```cpp
class Solution {
public:
    struct Entry {
        int i, j , sum;
        Entry(int _i, int _j, int _sum) : i(_i), j(_j), sum(_sum) {}
        bool operator< (const Entry& other) const {
            // 小根堆
            return sum > other.sum;
        }
    };

    // 返回大小为k的数组，这k个数即是目前为止计算出来的从mat的第0行到第j行（b的行数）
    // 的每一行选出一个元素的最小组合
    vector<int> merge(const vector<int>& a, const vector<int>& b, int k) {
        int m = a.size(), n = b.size();
        priority_queue<Entry> pq;
        for (int i = 0; i < min(k, m); i++) {
            pq.emplace(i, 0 , a[i] + b[0]);
        }

        vector<int> ans;
        while (k-- > 0 && !pq.empty()) {
            auto entry = pq.top();
            pq.pop();
            ans.push_back(entry.sum);
            if (entry.j + 1 < n) {
                pq.emplace(entry.i, entry.j + 1, a[entry.i] + b[entry.j + 1]);
            }
        }

        return ans;
    }

    int kthSmallest(vector<vector<int>>& mat, int k) {
        int m = mat.size(), n = mat[0].size();
        auto prev = mat[0];
        for (int i = 1; i < m; i++) {
            // 先求第0行和第1行的k个最小的数组合，然后依次往下求，最终prev[k-1]即为所求
            prev = move(merge(prev, mat[i], k));
        }
        return prev[k-1];
    }

};
```