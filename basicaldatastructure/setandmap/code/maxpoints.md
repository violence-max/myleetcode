题目描述：  
![image](/basicaldatastructure/setandmap/image/image5.png)  
解题过程：  
又干了一道hard，不过这道很弱。一开始我还想着自定义一个edge来深度优先搜索，后面发现没有必要，只要对于每一个点，都求出对于其它所有点的斜率，对于同一个斜率进行继续，斜率相同点的个数加1，然后求斜率相同的最大值就可以了。  
题解写得很细，记录一下  
代码：（我的→题解）  
```cpp
class Solution {
public:
    double getk(int x1, int y1, int x2, int y2) {
        return (double)(y2 - y1) / (x2 - x1);
    }

    int maxPoints(vector<vector<int>>& points) {
        int num = points.size();
        if (num <= 2) return num;
        vector<unordered_map<double,int>> count (num);
        for (int i = 0; i < num; i++) {
            int xi = points[i][0], yi = points[i][1];
            for (int j = i + 1; j < num; j++) {
                int xj = points[j][0], yj = points[j][1];
                double k = getk(xi,yi,xj,yj);
                count[i][k]++;
                count[j][k]++;
            }
        }
        int ans = 1;
        for (int i = 0; i < num; i++) {
            int result = 1;
            for (auto [k,cnt] : count[i]) {
                result = max(result,cnt+1);
            }
            ans = max(ans,result);
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }

    int maxPoints(vector<vector<int>>& points) {
        int n = points.size();
        if (n <= 2) {
            return n;
        }
        int ret = 0;
        for (int i = 0; i < n; i++) {
            if (ret >= n - i || ret > n / 2) {
                break;
            }
            unordered_map<int, int> mp;
            for (int j = i + 1; j < n; j++) {
                int x = points[i][0] - points[j][0];
                int y = points[i][1] - points[j][1];
                if (x == 0) {
                    y = 1;
                } else if (y == 0) {
                    x = 1;
                } else {
                    if (y < 0) {
                        x = -x;
                        y = -y;
                    }
                    int gcdXY = gcd(abs(x), abs(y));
                    x /= gcdXY, y /= gcdXY;
                }
                mp[y + x * 20001]++;
            }
            int maxn = 0;
            for (auto& [_, num] : mp) {
                maxn = max(maxn, num + 1);
            }
            ret = max(ret, maxn);
        }
        return ret;
    }
};
```