题目描述：

![image](/basical/array/image/image82.png)

解决过程：

5分钟ac

代码：

```
class Solution {
public:
    int pivotInteger(int n) {
        vector<int> v (n);
        iota(v.begin(), v.end(), 1);
        int sum = accumulate(v.begin(), v.end(), 0);
        int suffix = 0;
        for (int i = n - 1; i >= 0; i--) {
            suffix += v[i];
            if (sum == suffix) {
                return i + 1;
            } else {
                sum -= v[i];
            }
            cout << i << ' ' << suffix << ' ' << sum << endl;
        }
        return -1;
    }
};
```