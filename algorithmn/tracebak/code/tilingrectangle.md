题目描述：

![image](/algorithmn/tracebak/image/image26.png)

解题过程：

没做出来

- 回溯

代码：

```cpp
class Solution {
public:
    int ans;
    int tilingRectangle(int n, int m) {
        ans = n * m;
        vector<vector<bool>> visited (n, vector<bool> (m, false));
        dfs(visited, 0, 0, 0);
        return ans;
    }

    void dfs(vector<vector<bool>>& visited, int x, int y, int cnt) {
        //从坐标(x,y)开始暴力搜索可能的铺瓷砖方案，最终需要的正方形瓷砖个数记录为cnt

        int n = visited.size(), m = visited[0].size();
        if (cnt >= ans) {
            return; // 只需要总数更小的答案
        } else if (x >= n) {
            ans = cnt;
            return;
        } else if (y >= m) {
            dfs(visited, x + 1, 0, cnt); // 遍历下一行
            return;
        } else if (visited[x][y]) {
            dfs(visited, x, y + 1, cnt); // 遍历下一列
            return;
        }

        for (int k = min(n - x, m - y) /*贪心求得可以取到的面积最大的正方形长度*/; k >= 1 && isAvailable(visited, x, y, k); --k) {
            fillUp(visited, x, y, k, true);
            dfs(visited, x, y + k, cnt + 1); // 跳过k行
            fillUp(visited, x, y, k, false);
        }

        return;
    }

    bool isAvailable(const vector<vector<bool>>& visited, int x, int y, int k) {
        // 判断以坐标(x,y)为左上顶点的边长为k的正方形区域内是否有值为true的点，有则返回false

        for (int i = 0; i < k; ++i) {
            if (accumulate(visited[x + i].begin() + y, visited[x + i].begin() + y + k, false, bit_or())) {
                //bit_or()用于计算两数按位或

                return false;
            }
        }

        return true;
    }

    void fillUp(vector<vector<bool>>& visited, int x, int y, int k, bool val) {
        // 将以坐标(x,y)为左上顶点的边长为k的正方形区域的值都填为val
        for (int i = 0; i < k; i++) {
            fill(visited[x + i].begin() + y, visited[x + i].begin() + y + k, val);
        }
    }
};
```