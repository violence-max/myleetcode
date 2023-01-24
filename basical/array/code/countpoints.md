题目描述：  
![image](/basical/array/image/image40.png)  
解决过程：  
2分钟ac  
代码：  
```cpp
class Solution {
public:
    vector<int> countPoints(vector<vector<int>>& points, vector<vector<int>>& queries) {
       int n = queries.size();
       vector<int> ans (n);
       for (int i = 0; i < n; i++) {
           int x = queries[i][0], y = queries[i][1], z = queries[i][2];
           int len = z * z;
           int cnt = 0;
           for (auto& point : points) {
               int row = point[0], col = point[1];
               if ((row - x) * (row - x) + (col - y) * (col - y) <= len) {
                   cnt++;
               }
           }
           ans[i] = cnt;
       } 
       return ans;
    }
};
```