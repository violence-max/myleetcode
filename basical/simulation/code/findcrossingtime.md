题目描述：  

![image](/basical/simulation/image/image1.png)

解决过程：

没做出来

这是一道复杂模拟题，重点思路有以下几个：

1. 分析工人的四个状态，维持四个优先队列的运行
2. 优先队列结合自定义排序存储数组索引值，工作队列存储工作结束时间
3. 时间记录着工人的过桥时间，等同于存储了最终答案

代码：

```cpp
class Solution {
public:
    int findCrossingTime(int n, int k, vector<vector<int>>& time) {
        auto cmp = [&](int a, int b) {
            int timea = time[a][0] + time[a][2];
            int timeb = time[b][0] + time[b][2];
            return timea != timeb ? timea < timeb : a < b;
        };
        priority_queue<int, vector<int>, decltype(cmp)> waitleft(cmp), waitright(cmp);
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> workleft, workright;
        int t = 0;
        for (int i = 0; i < k; ++i) {
            waitleft.push(i);
        }
        while (n > 0 || !waitright.empty() || !workright.empty()) {
            while (!workleft.empty() && workleft.top().first <= t) {
                waitleft.push(workleft.top().second);
                workleft.pop();
            }
            while (!workright.empty() && workright.top().first <= t) {
                waitright.push(workright.top().second);
                workright.pop();
            }            

            if (!waitright.empty()) {
                int id  = waitright.top();
                waitright.pop();
                t += time[id][2];
                workleft.push({t + time[id][3], id});
            } else if (n > 0 && !waitleft.empty()) {
                int id = waitleft.top();
                waitleft.pop();
                t += time[id][0];
                workright.push({t + time[id][1], id});
                --n;
            } else {
                int nexttime = INT_MAX;
                if (!workleft.empty()) {
                    nexttime = min(nexttime, workleft.top().first);
                }
                if (!workright.empty()) {
                    nexttime = min(nexttime, workright.top().first);
                }
                t = nexttime;
            }
        }
        return t;
    }
};
```