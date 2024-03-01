题目描述：

![image](/basical/simulation/image/image13.png)

解决过程：

第360场周赛t1，不会做我靠，脑筋急转弯居然没有想出来

代码：

```cpp
class Solution {
public:
    int furthestDistanceFromOrigin(string moves) {
        int pos = 0, len = 0;
        for (char c : moves) {
            if (c == 'L') {
                --pos;
            } else if (c == 'R') {
                ++pos;
            } else {
                ++len;
            }
        }
        return abs(pos) + len;
    }
};
```