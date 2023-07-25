题目描述：

![image](/basical/simulation/image/image11.png)

解决过程：

模拟

代码：

```cpp
class Solution {
public:
    int halveArray(vector<int>& nums) {
        priority_queue<double> q;
        for (auto i : nums) q.push((double)i);
        int ans = 0;
        long long sum = 0;
        for (auto i : nums) sum += i;
        long double t = (double)sum / 2.0, tmps = 0.0;
        while (tmps < t) {
            double x = q.top();
            tmps += x / 2.0;
            ans++;
            q.pop();
            q.push(x / 2.0);
        }
        return ans;
    }
};
```