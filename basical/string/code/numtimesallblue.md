题目描述：

![image](/basical/string/image/image79.png)

解决过程：

5分钟ac

代码：

```cpp
class Solution {
public:
    int numTimesAllBlue(vector<int>& flips) {
        int n = flips.size();
        // vector<int> indices (n);
        // iota(indices.begin(), indices.end(), 0);
        // sort(indices.begin(), indices.end(), [&](int a, int b){
            // return flips[a] < flips[b];
        // });
        int remote = -1;
        int cnt = 0;
        for (int i = 0; i < n; ++i) {
            remote = max(remote, flips[i]); // 记录最远的翻转位置
            if (remote == i + 1) {
                // 如果当前的翻转位置和最远的翻转位置相等，则答案加1
                cnt++;
            }
        }
        return cnt;
    }
};
```