题目描述：  
![image](/algorithmn/greed/image/image11.png)  
解题过程：  
大概经历了三个过程：排序过后左边界比之前的一个右边界小则不需要加一，否则答案加一 →使用一个pre数值把最能抗的右边界记住→使用一个范围把能一箭射爆的几个气球包裹住  
题解里面排序直接按照右边界升序排序，是我这种没有逆向思维的人永远也想不到的。  
代码：  
```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(),points.end(),[](const vector<int>& u,const vector<int>& v) {
            return u[0] < v[0] || (u[0] == v[0] && u[1] <= v[1]);
        });
        int n = points.size();
        int ret = 1;
        int rightEdge = points[0][1],leftEdge = points[0][0];
        for (int i = 1; i < n; i++) {
            if (points[i][0] <= rightEdge) {
                leftEdge = points[i][0];
                if (points[i][1] <= rightEdge) {
                    rightEdge = points[i][1];
                }
            } else {
                leftEdge = points[i][0]; rightEdge = points[i][1];
                ret += 1;
            }
        }
        return ret;
    }
};
```