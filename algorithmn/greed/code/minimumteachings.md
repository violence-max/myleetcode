题目描述：

![image](/algorithmn/greed/image/image24.png)

解题过程：

没做出来，感觉是一种显式贪心

代码：

```cpp
class Solution {
public:
    int minimumTeachings(int n, vector<vector<int>>& languages, vector<vector<int>>& friendships) {
        unordered_set<int> s;
        for (const auto& f : friendships) {
            bool find = false;
            for (const auto& l1 : languages[f[0] - 1]) {
                for (const auto& l2 : languages[f[1] - 1]) {
                    if (l1 == l2) {
                        find = true;
                        break;
                    }
                }
                if (find) {
                    break;
                }
            }
            if (!find) {
                s.insert(f[0]);
                s.insert(f[1]);
            }
        }
        unordered_map<int,int> p;
        for (auto q : s) {
            for (auto i : languages[q-1]) {
                p[i]++;
            }
        }
        int mxx = 0;
        for (auto x : p) {
            mxx = max(mxx, x.second);
        }
        return s.size() - mxx;
    }
};
```