题目描述：

![image](/algorithmn/dynamic_programming/image/image66.png)

解决过程：

没做出来

- 状态压缩+动态规划：
    1. 引用减少代码复杂度
    2. memset的使用
    3. sizeof的括号是可选的
    4. 3进制编码动态规划
    5. 记忆化搜索
    6. static constexpr的使用
- 轮廓线dp

代码：（状态压缩+动态规划）

```cpp
class Solution {
private:
    static constexpr int T = 243, N = 5, M = 6;
    int mask_bits[T][N];
    int iv_count[T], ev_count[T];
    int inner_score[T], inter_score[T][T];
    int m, n, tot;
    int d[N][T][M+1][M+1];

    static constexpr int score[3][3] = {
        {0, 0, 0},
        {0, -60, -10},
        {0, -10, 40}
    };

public:
    void init_data() {
        memset(iv_count, 0, sizeof iv_count);
        memset(ev_count, 0, sizeof ev_count);
        memset(inner_score, 0, sizeof inner_score);
        // for (int i = 0; i < tot; ++i) {
        //     cout << inner_score[i] << ' ';
        // }
        // cout << endl;
        memset(inter_score, 0, sizeof inter_score);
        for (int mask = 0; mask < tot; ++mask) {
            int temp_mask = mask;
            for (int i = 0; i < n; ++i) {
                int x = temp_mask % 3;
                mask_bits[mask][i] = x;
                temp_mask /= 3;
                if (x == 1) {
                    iv_count[mask]++;
                    // cout << origin_mask << ' ';
                    inner_score[mask] += 120;
                } else if (x == 2) {
                    ev_count[mask]++;
                    inner_score[mask] += 40;
                }
                if (i > 0) {
                    inner_score[mask] += score[mask_bits[mask][i-1]][x];
                }
            }
            // cout << mask << ' ';
            // for (int i = 0; i < n; ++i) {
            //     cout << mask_bits[mask][i] << ' ';
            // }
            // cout << inner_score[mask] << endl;
        }

        for (int i = 0; i < tot; ++i) {
            for (int j = 0; j < tot; ++j) {
                for (int k = 0; k < n; ++k) {
                    inter_score[i][j] += score[mask_bits[i][k]][mask_bits[j][k]];
                }
            }
        }
    }

    int dfs(int row, int premask, int iv, int ev) {
        if (row == m || (iv <= 0 && ev <= 0)) {
            return 0;
        }
        // cout << row << ' ' << premask << ' ' << iv << ' ' << ev << endl;
        if (d[row][premask][iv][ev] != -1) {
            return d[row][premask][iv][ev];
        }
        int& res = d[row][premask][iv][ev];
        for (int mask = 0; mask < tot; ++mask) {
            if (iv - iv_count[mask] < 0 || ev - ev_count[mask] < 0) {
                continue;
            }
            res = max(res, dfs(row + 1, mask, iv - iv_count[mask], ev - ev_count[mask]) +
                            inner_score[mask] +
                            inter_score[premask][mask]);
            // cout << row << ' ' << premask << ' ' << mask << ' ' << inner_score[mask] << ' ' << inter_score[premask][mask] << ' ' << res << endl;
        }
        return res;
    }

    int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {
        this->m = m;
        this->n = n;
        tot = pow(3, n);
        init_data();
        memset(d, -1, sizeof d);
        return dfs(0, 0, introvertsCount, extrovertsCount);
    }
};
```