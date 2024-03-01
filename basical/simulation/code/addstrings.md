题目描述：

![image](/basical/simulation/image/image4.png)

解决过程：

5分钟ac

代码：

```cpp
class Solution {
public:
    string addStrings(string num1, string num2) {
        int c = 0;
        int n = num1.size(), m = num2.size();
        int i = n - 1, j = m - 1;
        string ans;
        while (i >= 0 || j >= 0 || c > 0) {
            int x;
            if (i >= 0) {
                x = num1[i] - '0';
                i--;
            } else {
                x = 0;
            }
            int y;
            if (j >= 0) {
                y = num2[j] - '0';
                j--;
            } else {
                y = 0;
            }
            // cout << x << ' ' << y << ' ' << c << endl;
            int p = x + y + c;
            c = p / 10;
            int q = p % 10;
            ans += q + '0';
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```