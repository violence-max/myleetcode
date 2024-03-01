题目描述：

![image](/basical/IQ/image/image46.png)

解决过程：

没做出来
代码：

```cpp
class Solution {
public:
    bool checkOverlap(int radius, int xCenter, int yCenter, int x1, int y1, int x2, int y2) {
        long long dist = 0;
        if (xCenter < x1 || xCenter > x2) {
            // 圆心不在两条竖边范围内
            dist += min(pow(x1 - xCenter, 2), pow(x2 - xCenter, 2));
        }        
        if (yCenter < y1 || yCenter > y2) {
            dist += min(pow(y1 - yCenter, 2), pow(y2 - yCenter, 2));
        }
        return dist <= radius * radius;
    }
};
```