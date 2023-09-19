题目描述：

![image](/basical/array/image/image109.png)

解决过程：

第113场双周赛t3，没做出来

其实想到了两数之和，但是异或不会操作，没想到只是变个样子而已，还得再练习举一反三才行

代码：

```cpp
class Solution {
public:
    int countPairs(vector<vector<int>>& coordinates, int k) {
        int n = coordinates.size();
        function<long long(int, int)> get = [&](int x, int y) -> long long {
            return (long long)x * 1e6 + y;
        };
        unordered_map<long long, int> s;
        int ret = 0;
        for (int i = 0; i < n; ++i) {
            int x = coordinates[i][0], y = coordinates[i][1];
            for (int j = 0; j <= k; ++j) {
                int xx = j ^ x, yy = (k-j) ^ y;
                if (s.count(get(xx,yy))) ret += s[get(xx,yy)];
            }
            s[get(x,y)]++;
        }
        return ret;
    }
};
```