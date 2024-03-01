题目描述：

![image](/basicaldatastructure/stackandquere/image/image26.png)

解决过程：

第351场周赛t4，肯定会做，但是卡在t2了

- 单调栈

代码：

```cpp
class Solution {
public:
    vector<int> survivedRobotsHealths(vector<int>& positions, vector<int>& healths, string directions) {
        vector<int> ans;
        int n = positions.size();
        vector<int> indices (n);
        iota(indices.begin(), indices.end(), 0);
        sort(indices.begin(), indices.end(), [&](int a, int b){
            return positions[a] < positions[b];
        });
        stack<int> stk;
        // for (int i = 0; i < indices.size(); ++i) {
        //     cout << indices[i] << ' ';
        // }
        for (int i : indices) {
            if (directions[i] == 'R') {
                // cout << "the position " << i << " is push_back into stk" << endl;
                stk.push(i);
            } else {
                while (!stk.empty() && healths[i] > 0) {
                    int temp = healths[i];
                    healths[i] -= healths[i] > healths[stk.top()] ? 1 : healths[i];
                    healths[stk.top()] -= healths[stk.top()] > temp ? 1 : healths[stk.top()];
                    // cout << i << ' ' << healths[i] << ' ' << healths[stk.top()] << endl;
                    if (healths[stk.top()] == 0) {
                        stk.pop();
                    }
                    
                }
            }
        }
        for (int h : healths) {
            if (h != 0) {
                ans.push_back(h);
            }
        }
        return ans;
    }
};
```