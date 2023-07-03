题目描述：

![image](/basical/matrix/image/image17.png)

解决过程：

没做出来

- 分治+四分

代码：（四分）

```cpp
class Solution {
public:
    int countShips(Sea sea, vector<int> topRight, vector<int> bottomLeft) {
        int x1 = topRight[0], y1 = topRight[1];
        int x2 = bottomLeft[0], y2 = bottomLeft[1];

        if (x1 < x2 || y1 < y2 || !sea.hasShips(topRight, bottomLeft)) {
            // 不构成矩形或者没有船只
            return 0;
        }

        if (x1 == x2 && y1 == y2) {
            // 找到了一个点，即一艘船
            return 1;
        }
        
        int xmid = (x1 + x2) / 2, ymid = (y1 + y2) / 2;
        // 四分的矩形互不相交，并且保证每个点都在查询范围内
        // 最终搜索的目标仅仅是一个点
        return countShips(sea, {x1, y1}, {xmid + 1, ymid + 1}) +
               countShips(sea, {x1, ymid}, {xmid + 1, y2}) + 
               countShips(sea, {xmid, ymid}, {x2, y2}) + 
               countShips(sea, {xmid, y1}, {x2, ymid + 1});
    }
};
```