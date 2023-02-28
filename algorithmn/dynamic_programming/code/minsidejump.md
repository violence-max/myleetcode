题目描述：  
![image](/algorithmn/dynamic_programming/image/image43.png)  
解题过程：  
想不出动态规划怎么写，于是用回溯，果不其然超时了，没办法，数据量都五位数了，必然超时的。  
这应该属于状态压缩的题目，题解用dp[i][j]来表示青蛙跳到i点时在j跑道上已经使用的最少侧跳次数，起初时因为小青蛙在2跑道，因此dp[0][1]赋值为0，而dp[0][0]和dp[0][2]均赋值为1（因为到达该点需要从2跑道发生一次侧跳）。然后青蛙往前走，如果有哪个点上 有一个跑道有障碍物，就把这个点对应的跑道需要的侧跳次数设置成无穷大，否则用MIN来记录在该点上需要的最小侧跳次数，然后尝试用MIN+1来尝试对该点上三个跑道的最少侧跳次数进行赋值。这里是最精髓的。假设MIN是由跑道2赋值的 ，说明在该点上跑道2需要的侧跳次数最少，所以在尝试更新时不会对跑道2上的值进行赋值，而其余两个跑道需要重新赋值（因为需要从跑道2侧跳一次过去）。就是这样，不断地发生状态转移后就可以在最后一个点的三个跑道上取最小值即可  
灵神写了0-1BFS的方法，虽然说过年不太想看，但是为了尊重自己，还是记下来了，感觉这个方法有点笨重，每个可行节点都判断了一次。细节就不说那么多了，见代码吧，我自己也是模拟了一次才大概知道这个算法在干嘛。主要的就是如果可以向右走就连接一条权值为0的边，需要侧跳就连接一条权值为1的边  
代码：（动态规划→0-1BFS）  
```cpp
class Solution {
public:
    int minSideJumps(vector<int>& obstacles) {
        int n = obstacles.size() - 1;
        vector<int> dp = {1,0,1};
        for (int i = 1; i <= n; i++) {
            int MIN = INT_MAX;
            for (int j = 0; j < 3; j++) {
                if (j == obstacles[i] - 1) {
                    dp[j] = INT_MAX;
                } else {
                    MIN = min(MIN,dp[j]);
                }
            }
            for (int j = 0; j < 3; j++) {
                if (j == obstacles[i] - 1) {
                    continue;
                }
                dp[j] = min(dp[j],MIN+1);
            }
        }
        return *min_element(dp.begin(),dp.end());
    }
};
```
```cpp
class Solution {
public:
    int minSideJumps(vector<int> &obstacles) {
        int n = obstacles.size(), dis[n][3];
        memset(dis, 0x3f, sizeof(dis));
        dis[0][1] = 0;
        deque<pair<int, int>> q;
        q.emplace_back(0, 1); // 起点
        for (;;) {
            auto[i, j] = q.front(); q.pop_front();
            int d = dis[i][j];
            if (i == n - 1) return d; // 到达终点
            if (obstacles[i + 1] != j + 1 && d < dis[i + 1][j]) { // 向右
                dis[i + 1][j] = d;
                q.emplace_front(i + 1, j); // 加到队首
            }
            for (int k : {(j + 1) % 3, (j + 2) % 3}) // 枚举另外两条跑道（向上/向下）
                if (obstacles[i] != k + 1 && d + 1 < dis[i][k]) {
                    dis[i][k] = d + 1;
                    q.emplace_back(i, k); // 加到队尾
                }
        }
    }
};
```