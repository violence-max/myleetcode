题目描述：

![image](/basical/IQ/image/image47.png)

解决过程：

第350场周赛t1

代码：

```cpp
class Solution {
public:
    int distanceTraveled(int mainTank, int additionalTank) {
        int res = 0;
        while (mainTank) {
            if (mainTank >= 5) {
                res += 50;
                if (additionalTank > 0) {
                    mainTank++;
                    additionalTank--;
                }
                mainTank -= 5;
            } else {
                res += mainTank * 10;
                mainTank = 0;
            }
        }
        return res;
    }
};
```