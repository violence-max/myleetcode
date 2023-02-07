题目描述：  
![image](/basical/array/image/image50.png)  
解决过程：  
写了挺久的吧，最终用自定义排序+优先队列写出来了，而且确定了以开始时间为先的排序方式。看了题解，另外一种写法是双指针，我觉得非常不错  
代码：（优先队列→双指针）  
```cpp
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),[](vector<int> i, vector<int> j) {
            return i[0] < j[0] || (i[0] == j[0] && i[1] < j[1]);
        });
        int res = 0, cnt = 0, index = 0;
        int n = intervals.size();
        priority_queue<int,vector<int>,greater<int>> q;
        while (index < n) {
            int _start = intervals[index][0], _end = intervals[index][1];
            while (!q.empty() && _start >= q.top()) {
                q.pop();
                cnt++;
            }
            if (cnt) {
                cnt--;
            } else {
                res++;
            }
            q.push(_end);
            index++;
        }
        return res;
    }
};
```  
```cpp
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        int m = intervals.size();
        vector<int> start (m), end (m);
        for (int i = 0; i < m; i++) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        sort(start.begin(),start.end());
        sort(end.begin(),end.end());
        int s_ptr = 0, e_ptr = 0, usedroom = 0;
        while (s_ptr < m) {
            if (start[s_ptr] >= end[e_ptr]) {
                e_ptr++;
            } else usedroom++;
            s_ptr++;
        }
        return usedroom;
    }
};
```