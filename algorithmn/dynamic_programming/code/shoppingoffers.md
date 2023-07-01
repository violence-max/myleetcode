题目描述：

![image](/algorithmn/dynamic_programming/image/image71.png)

解决过程：
一开始没注意到“不能购买超出购物清单指定数量的物品这句话”，并且在位运算这一块知识比较欠缺，做得比较慢而且对于res的初始化—不买任何礼包，也没有思考正确。看了题解答案些许代码之后改了一下就出来了。

代码：（状态压缩dp）

```cpp
class Solution {
public:
    int shoppingOffers(vector<int>& price, vector<vector<int>>& special, vector<int>& needs) {
        int len = special.size(), n = needs.size();
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            mask = mask << 4;
            mask |= needs[i];
        }
        int memo[1 << 24];
        memset(memo, -1, sizeof memo);
        memo[0] = 0;
        // cout << "here" << endl;
        int mt = 15;
        function<int(int)> dfs = [&](int masks) -> int {
            if (memo[masks] != -1) {
                return memo[masks];
            }
            int res = 0, m = 0;
            for (int i = n - 1; i >= 0; --i) {
                res += ((masks >> m) & mt) * price[i];
                m += 4;
            }
            for (const auto& spe : special) {
                int newmask = 0;
                int temp = 0;
                bool flag = true;
                for (int j = n - 1; j >= 0; --j) {
                    int cnt = (masks >> temp) & mt;
                    if (spe[j] > cnt) {
                        flag = false;
                        break;
                    }
                    newmask = newmask | ((cnt - spe[j]) << temp);
                    temp += 4;
                }
                if (flag) {
                    res = min(res, dfs(newmask) + spe[n]);
                }
            }
            return memo[masks] = res;
        };
        return dfs(mask);
    }
};
```