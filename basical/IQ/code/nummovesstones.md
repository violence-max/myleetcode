题目描述：  
![image](/basical/IQ/image/image33.png)  
解决过程：  
没做出来  
- 思维题，直接看代码  
代码：  
```cpp
class Solution {
public:
    vector<int> numMovesStonesII(vector<int>& stones) {
        int n = stones.size();
        sort(stones.begin(), stones.end()); // 排序预处理
        if (stones.back() - stones[0] + 1 == n) {
            // 刚好占满了n个格子
            return {0, 0};
        }
        // 每次从位置0或者位置n - 2开始进行往中间的最优移动，则最大移动次数为剩下的n-1颗石子之间的空格数，即距离减去n-1颗目前的非空石子
        int ma = max(stones[n - 2] - stones[0] + 1, stones[n - 1] - stones[1] + 1) - (n - 1);
        int mi = n;
        // 选取最小的石子，以长度为n且含有m颗石子的例子对m进行讨论分析
        for (int i = 0, j = 0; i < n && j + 1 < n; ++i) {
            // 总时选取石子个数最多的窗口
            while (j + 1 < n && stones[j + 1] - stones[i] + 1 <= n) {
                // 保证退出循环之后的j满足stones[j] - stones[i] + 1 <= n，即在区间[i,j]之内的石子总数小于等于n
                ++j;
            }
            // 区间内有n - 1颗石子且空位位于端点处可以取到最小操作数2（忘了怎么取就去看题解，不好描述）
            if (j - i + 1 == n - 1 && stones[j] - stones[i] + 1 == n - 1) {
                mi = min(mi, 2);
            } else {
                // 合并了区间内的石子个数为n为小于等于n - 2的情况：最终都能取到n - m的最小操作数
                // 先用区间外的石子把区间内的空位给填满，然后区间外的石子逐一往回收即可：a，b，c，d逐一往回收
                // 由于区间外的每个石子只移动一次，因此最小操作数为n - m
                mi = min(mi, n - (j - i + 1));
            }
        }
        return {mi, ma};
    }
};
```