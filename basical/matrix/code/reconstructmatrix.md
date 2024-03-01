题目描述：

![image](/basical/matrix/image/image16.png)

解决过程：

一开始想回溯解决，后来发现可以直接赋值，做了大概二十分钟吧

代码：

```cpp
class Solution {
public:
    vector<vector<int>> reconstructMatrix(int upper, int lower, vector<int>& colsum) {
        int n = colsum.size();
        vector<vector<int>> ans (2, vector<int> (n));
        function<void(int, int, int)> assign = [&](int val1, int val2, int index) -> void {
            ans[0][index] = val1;
            ans[1][index] = val2;
        };
        int cnt = 0;
        for (int i = 0; i < n; ++i) {
            if (colsum[i] == 0) {
                assign(0, 0, i);
            } else if (colsum[i] == 2) {
                assign(1, 1, i);
                upper--;
                lower--;
            } else {
                cnt++;
            }
        }
        if (upper < 0 || lower < 0 || upper + lower != cnt) {
            return {};
        }
        for (int i = 0; i < n; ++i) {
            if (colsum[i] != 1) {
                continue;
            }

            if (upper > 0) {
                assign(1, 0, i);
                upper--;
            } else {
                assign(0, 1, i);
            }
        }
        return ans;
    }
};
```