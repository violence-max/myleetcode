题目描述：

![image](/basicaldatastructure/binary_tree/image/image64.png)

解决过程：

第360场周赛t4，没做出来

树上倍增的一道裸题

代码：

```cpp
long long sum[100020][41];
int goal[100020][41];
long long c[41];

class Solution {
public:
    long long getMaxFunctionValue(vector<int>& receiver, long long k) {
        int n = receiver.size();
        for (int i = 0; i < n; ++i) {
            goal[i][0] = receiver[i];
            sum[i][0] = i + receiver[i];
        }
        c[0] = 1;
        for (int i = 1; i < 41; ++i) {
            c[i] = c[i-1] << 1;
            for (int j = 0; j < n; ++j) {
                goal[j][i] = goal[goal[j][i-1]][i-1];
                sum[j][i] = sum[j][i-1] + sum[goal[j][i-1]][i-1] - goal[j][i-1];
            }
        }

       long long ans = 0;
       long long aug;
        for (int i = 0; i < n; ++i) {
            aug = i;
            int now = i;
            for (int j = 40; j >= 0; --j) {
                if (k & c[j]) {
                    aug -= now;
                    aug += sum[now][j];
                    now = goal[now][j];
                }
            }
            ans = max(ans, aug);
        }
        return ans;
    }
};
```