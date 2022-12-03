题目描述： 
![image](/algorithmn/greed/image/image12.png)  
解题过程：  
因为做过拉弓射气球的题目，这个题目还是太简单了，6分钟就搞定了。  
看了题解，居然还是拿右边界进行排序，牛逼。  
代码：  
```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),[](const vector<int> &u,const vector<int> &v){
            return u[0] < v[0] || (u[0] == v[0] && u[1] < v[1]);
        });
        int n = intervals.size();
        int ret = 0;
        int left = intervals[0][0],right = intervals[0][1];
        for (int i = 1; i < n; i++) {
            if (intervals[i][0] < right) {
                left = intervals[i][0];
                if (intervals[i][1] <= right) right = intervals[i][1];
                ret += 1;
            } else {
                left = intervals[i][0];
                right = intervals[i][1];
            }
        }
        return ret;
    }
};
```