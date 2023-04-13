题目描述：  
![image](/basical/IQ/image/image32.png)  
解题过程：  
没做出来  
- 分析法：
1. 从位移方面考虑，机器人完成一次指令以后的朝向，最终得出的结论是：只有机器人朝向北的时候才不会形成环
2. 模拟：用direction数组模拟顺时针旋转，将指令L视为逆时针旋转，R视为顺时针旋转  
代码：  
```cpp
class Solution {
public:
    bool isRobotBounded(string instructions) {
        vector<vector<int>> direc {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        int direcIndex = 0;
        int x = 0, y = 0;
        for (char instruction : instructions) {
            if (instruction == 'G') {
                x += direc[direcIndex][0];
                y += direc[direcIndex][1];
            } else if (instruction == 'L') {
                direcIndex += 3;
                direcIndex %= 4;
            } else {
                direcIndex++;
                direcIndex %= 4;
            }
        }
        return direcIndex != 0 || (x == 0 && y == 0);
    }
};
```