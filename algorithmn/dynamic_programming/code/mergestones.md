题目描述：  
![image](/algorithmn/dynamic_programming/image/image57.png)  
解题过程：  
没做出来  
- 动态规划  
代码：  
```cpp
class Solution {
    static constexpr int inf = 0x3f3f3f3f;
    vector<vector<vector<int>>> d;
    vector<int> sum;
    int k;
public:
    int get(int l, int r, int t) {
        // 若 d[l][r][t] 不为 -1，表示已经在之前的递归被求解过，直接返回答案
        if (d[l][r][t] != -1) {
            return d[l][r][t];
        }
        // 当石头堆数小于 t 时，一定无解
        if (t > r - l + 1) {
            return inf;
        }
        if (t == 1) {
            int res = get(l, r, k); // 要得到1堆石头，先得到k堆
            if (res == inf) {
                return d[l][r][t] = inf;
            }
            // res 加上 k堆石头合并的成本
            return d[l][r][t] = res + (sum[r] - (l == 0 ? 0 : sum[l - 1]));
        }
        int val = inf;
        for (int p = l; p < r; p += (k - 1)) {
            // 分成[l,p]和[p+1,r] 两个区间
            // 递归求解：分别求解一堆石头和t-1堆石头
            val = min(val, get(l, p, 1) + get(p + 1, r, t - 1));
        }
        return d[l][r][t] = val;
    }
    int mergeStones(vector<int>& stones, int k) {
        int n = stones.size();
        if ((n - 1) % (k - 1) != 0) {
            // 每次移动少k - 1堆石头，假设移动x次可以得到一堆石子，则：n - x(k-1) = 1
            return -1;
        }
        this->k = k;
        //  dp[l][r][t]表示区间[l,r]内合并成t堆石子所需的最低成本
        // 0 <= t <= k，最大合并成k堆石子是最有意义的，因为可以从k堆石头得到一堆
        d = vector(n, vector(n, vector<int>(k + 1, -1))); 
        sum = vector<int>(n, 0);

        // 初始化
        for (int i = 0, s = 0; i < n; i++) {
            d[i][i][1] = 0; //数据预处理
            s += stones[i];
            sum[i] = s; // sum[i]表示0~i的石头总数
        }
        int res = get(0, n - 1, 1);
        return res;
    }
};
```