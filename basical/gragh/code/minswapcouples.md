题目描述：

![image](/basical/gragh/image/image13.png)

解决过程：

用贪心做了一个小时没做出来

- 并查集
- 广度优先搜搜
- 贪心

代码：（并查集→广度优先搜索→贪心）

```cpp
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        // 考虑现有的坐在一起的两对男女，且有对情侣被这一个在第一对男女，另一个在第二对男女中，则需要考虑两种需要换座位的情况：
        // 1. [a1,b1], [b2, a2]；2. [a1, b1], [b2, ..]
        // 使用并查集去考虑
        // a1->b1, b2->a2==a1->b1，此时有两条边，则进行一次座位交换即可
        // 第二种情况也是同理
        // 因此只需要获取并查集数组f，f中每对情侣指向了最终需要进行换座位的那对情侣
        // 即每个连通分量中边的数目减去1即可得到答案
        int n = row.size();
        int tot = n / 2;
        vector<int> f (tot);
        for (int i = 0; i < tot; ++i) {
            f[i] = i;
        }
        function<int(int)> get = [&](int x) -> int {
            if (f[x] == x) {
                return x;
            }
            return f[x] = get(f[x]);
        };
        function<void(int,int)> add = [&](int a, int b) -> void {
            int q = get(a);
            int p = get(b);
            f[q] = p;
        };
        // cout << "here" << endl;
        for (int i = 0; i < n; i += 2) {
            int first = row[i] / 2;
            int second = row[i + 1] / 2;
            add(first, second);
        }
        // cout << "hereV2" << endl;
        int res = 0;
        unordered_map<int, int> map;
        for (int i = 0; i < tot; ++i) {
            map[get(i)]++;
        }
        for (const auto& [u, sz] : map) {
            res += sz - 1;
        }
        return res;
    }
};
```

```cpp
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        int n = row.size();
        int tot = n / 2;
        
        vector<vector<int>> graph(tot);
        for (int i = 0; i < n; i += 2) {
            int l = row[i] / 2;
            int r = row[i + 1] / 2;
            if (l != r) {
                graph[l].push_back(r);
                graph[r].push_back(l);
            }
        }
        vector<int> visited(tot, 0);
        int ret = 0;
        for (int i = 0; i < tot; i++) {
            if (visited[i] == 0) {
                queue<int> q;
                visited[i] = 1;
                q.push(i);
                int cnt = 0;

                while (!q.empty()) {
                    int x = q.front();
                    q.pop();
                    cnt += 1;

                    for (int y: graph[x]) {
                        if (visited[y] == 0) {
                            visited[y] = 1;
                            q.push(y);
                        }
                    }
                }
                ret += cnt - 1;
            }
        }
        return ret;
    }
};
```

```cpp
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        int n = row.size();
        vector<int> pos (n);
        for (int i = 0; i < n; ++i) {
            pos[row[i]] = i;
        }
        int res = 0;
        for(int i = 1; i < n; i += 2) {
            int x = row[i] ^ 1;
            // cout << row[i - 1] << ' ' << row[i] << ' ' << x << endl;
            // if (row[i - 1] != x) {
               // cout << "aaa" << endl;
            // }
            while (row[i - 1] != (row[i] ^ 1)) {
                // cout << "here" << endl;
                int exchange_index = pos[row[i] ^ 1] ^ 1;
                // cout << exchange_index << endl;
                swap(row[i], row[exchange_index]);
                res++;
            }
        }
        return res;
    }
};
```