题目描述：  
![image](/basical/array/image/image64.png)  
解决过程：  
2分钟ac  
代码：  
```cpp
class Solution {
public:
    int maxWidthOfVerticalArea(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), [&](auto v, auto u){
            return v[0] < u[0];
        });
        int ans = 0;
        for (int i = 1; i < points.size(); i++) {
            ans = max(ans, points[i][0] - points[i-1][0]);
        }
        return ans;
    }
};
```