题目描述：

![image](/basicaldatastructure/stackandquere/image/image24.png)

解决过程：

做了两个小时

- 优先队列：
    1. pair<int,int>的哈希函数
    2. 带序号的自定义排序

代码：（我的→题解）

```cpp
class Solution {
public:
    struct Entry {
        int processtime;
        int index;
        int enqueuedtime; 
        Entry(int _processtime, int _index, int _enqueuedtime) : processtime(_processtime), index(_index), enqueuedtime(_enqueuedtime) {}

        bool operator> (const Entry& oths) const {
            return processtime > oths.processtime || (oths.processtime == processtime && index > oths.index);
        }
    };

    vector<int> getOrder(vector<vector<int>>& tasks) {
        int n = tasks.size();
        if (n == 1) {
            return {0};
        }

        vector<vector<int>> newtasks (n, vector<int> (3));
        for (int i = 0; i < n; ++i) {
            newtasks[i][0] = tasks[i][0];
            newtasks[i][1] = tasks[i][1];
            newtasks[i][2] = i; // 带着序号排序
        }
        sort(newtasks.begin(), newtasks.end());
        priority_queue<Entry, vector<Entry>, greater<Entry>> pq;
        vector<int> ans;
        int lastentime = newtasks[0][0]; // 上一个任务的入队时间
        int remotetime = INT_MAX; // 最远执行时间
        pq.push({newtasks[0][1], newtasks[0][2], newtasks[0][0]});
        for (int i = 1; i < n; ++i) {
            int enqueuedtime = newtasks[i][0], processtime = newtasks[i][1];
            if ((lastentime != enqueuedtime && remotetime == INT_MAX) || remotetime < enqueuedtime) {
                // 上一次入队时间小于当前入队时间或者需要考虑[remotetime, enqueuedtime)之间任务的执行情况

                while (!pq.empty() && (remotetime == INT_MAX || remotetime < enqueuedtime)) {
                    auto [x, y, z] = pq.top();
                    pq.pop();
                    ans.push_back(y);
                    remotetime = remotetime == INT_MAX ? z + x : remotetime > z ? remotetime + x : z + x;
                }
            }
            lastentime = enqueuedtime;
            pq.push({processtime, newtasks[i][2], enqueuedtime});
        }
        while (!pq.empty()) {
            auto [x, y, z] = pq.top();
            pq.pop();
            ans.push_back(y);
        }
        return ans;
    }
};
```

```cpp
class Solution {
private:
    using PII = pair<int, int>;
    using LL = long long;

public:
    vector<int> getOrder(vector<vector<int>>& tasks) {
        int n = tasks.size();
        vector<int> indices(n);
        iota(indices.begin(), indices.end(), 0);
        sort(indices.begin(), indices.end(), [&](int i, int j) {
            return tasks[i][0] < tasks[j][0];
        });

        vector<int> ans;
        // 优先队列
        priority_queue<PII, vector<PII>, greater<PII>> q;
        // 时间戳
        LL timestamp = 0;
        // 数组上遍历的指针
        int ptr = 0;
        
        for (int i = 0; i < n; ++i) {
            // 如果没有可以执行的任务，直接快进
            if (q.empty()) {
                timestamp = max(timestamp, (LL)tasks[indices[ptr]][0]);
            }
            // 将所有小于等于时间戳的任务放入优先队列
            while (ptr < n && tasks[indices[ptr]][0] <= timestamp) {
                q.emplace(tasks[indices[ptr]][1], indices[ptr]);
                ++ptr;
            }
            // 选择处理时间最小的任务
            auto&& [process, index] = q.top();
            timestamp += process;
            ans.push_back(index);
            q.pop();
        }
        
        return ans;
    }
};
```