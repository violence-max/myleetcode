题目描述：

![image](/basical/IQ/image/image48.png)

解决过程：

第352场周赛t2，不知道质数筛，没做出来

代码：（埃式筛）

```cpp
const int MX = 1e6;
vector<int> primes;
bool visited[MX + 1];
int init = []() {
    for (int i = 2; i * i <= MX; ++i) {
        if (!visited[i]) {
            for (int j = i * i; j <= MX; j += i) {
                visited[j] = true;
            }
        }
    }
    for (int i = 2; i <= MX; ++i) {
        if (!visited[i]) {
            primes.push_back(i);
        }
    }
    return 0;
}();
class Solution {
public:
    vector<vector<int>> findPrimePairs(int n) {
        cout << primes.size() << endl;
        vector<vector<int>> ans;
        for (int x : primes) {
            int y = n - x;
            // cout << x << ' ' << y << endl;
            if (y < x) {
                // 防止出现重复的数对
                break;
            }
            if (!visited[y]) {
                ans.push_back({x, y});
            }
        }
        return ans;
    }
};
```