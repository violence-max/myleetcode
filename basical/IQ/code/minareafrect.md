题目描述：

![image](/basical/IQ/image/image51.png)

解决过程：

两个小时没写出来，牢底坐穿

- 枚举三角形+向量

代码：

```cpp
class Solution {
public:
    double minAreaFreeRect(vector<vector<int>>& points) {
        int n = points.size();
        double ans = (double)INT_MAX;
        for (int i = 0; i < n - 2; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = i + 1; j < n - 1; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                for (int k = j + 1; k < n; ++k) {
                    int x3 = points[k][0], y3 = points[k][1];

                    int x4 = x2 + x3 - x1, y4 = y2 + y3 - y1;
                    for (const auto& point : points) {
                        if (x4 == point[0] && y4 == point[1]) {
                            int dot = (x2 - x1) * (x3 - x1) + (y2 - y1) * (y3 - y1);
                            if (dot == 0) {
                                double d1 = (double)sqrt(abs(x2 - x1) * abs(x2 - x1) + abs(y2 - y1) * abs(y2 -y1));
                                double d2 = (double)sqrt(abs(x3 - x1) * abs(x3 - x1) + abs(y3 - y1) * abs(y3 -y1));                          
                                double area = d1 * d2;
                                ans = min(ans, area);
                            }
                            break;
                        }
                    }
                }
            }
        }
        return ans == (double)INT_MAX ? 0 : ans;
    }
};
```