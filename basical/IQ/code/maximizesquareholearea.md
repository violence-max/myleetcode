题目描述：

![image](/basical/IQ/image/image64.png)

解决过程：

第118场双周赛t2，没做出来

```cpp
class Solution {
public:
    int f(vector<int>& a) { // 求出可以构造出的矩形的最长边长
        sort(a.begin(), a.end());
        int mx = 0, i = 0;
        int n = a.size();
        while (i < n) {
            int st = i;
            for (i++; i < n && a[i] - a[i-1] == 1; i++);
            // cout << st << ' ' << i << endl;
            mx = max(mx, i - st + 1); // 消除x条连续的边可以获得长度为x+1的边长
        }
        // cout << mx << endl;
        return mx;
    }

    int maximizeSquareHoleArea(int n, int m, vector<int>& hBars, vector<int>& vBars) {
        // int len1 = f(hBars);
        int len = min(f(hBars), f(vBars)); // 两者取最小值即可获得正方形的边长
        // cout << len << endl;
        return len * len;
    }
};
```