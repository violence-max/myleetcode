题目描述：

![image](/basical/matrix/image/image14.png)

解决过程：

竞赛题，没ac

- 逆向思维

代码：

```cpp
class Solution {
public:
    long long matrixSumQueries(int n, vector<vector<int>>& queries) {
        int sumX = 0, sumY = 0;
        vector<bool> xvisit (n, false);
        vector<bool> yvisit (n, false);
        int m = queries.size();
        long long ret = 0;
        for (int i = m - 1; i >= 0; i--) {
            int type = queries[i][0], x = queries[i][1], val = queries[i][2];
            if (type == 0) {
                if (xvisit[x]) continue;
                xvisit[x] = true;
                sumX++;
                long long aug = n - sumY;
                ret += aug * val;
            } else {
                if (yvisit[x]) continue;
                yvisit[x] = true;
                sumY++;
                long long aug = n - sumX;
                ret += aug * val;
            }
        }
        return ret;
    }
};
```