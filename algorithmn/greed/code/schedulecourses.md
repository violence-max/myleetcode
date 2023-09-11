题目描述：

![image](/algorithmn/greed/image/image28.png)

解决过程：

没做出来

- 贪心

```cpp
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        sort(courses.begin(), courses.end(), [&](const vector<int>& a, const vector<int>& b){
            return a[1] < b[1];
        });
        priority_queue<int> q;
        int t = 0;
        for (auto& x : courses) {
            int d = x[0], l = x[1];
            if (t + d <= l) {
                t += d;
                q.push(d);
            } else if (!q.empty() && q.top() > d) {
                t -= q.top() - d;
                q.pop();
                q.push(d);
            }
        }
        return q.size();
    }
};
```