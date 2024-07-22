题目描述：

![image](/basical/IQ/image/image65.png)

解决过程：

第407场周赛t2，没有做出来

脑筋急转弯

代码：

```cpp
class Solution {
public:
    bool doesAliceWin(string s) {
        for (auto c : s) {
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') return true;
        }
        return false;
    }
};
```