题目描述：  
![image](/basical/array/image/image51.png)  
解决过程：  
一开始想的是滑动窗口，然后发现不行，就用了动态规划，发现一维的不行，二维很暴力，肯定超时，最终看了题解。题解提供了两种方案，一种是前缀数组+栈，一种是哈希表。  
第一种是记录了每个从索引0开始到位置i的工作时长大于8的天数与工作时长小于等于8的天数之差（暂时不知道怎么描述）  
方法二也存疑  
代码：（前缀数组+栈→哈希表）  
```cpp
class Solution {
public:
    int longestWPI(vector<int>& hours) {
        int n = hours.size();
        stack<int> stk;
        stk.push(0);
        vector<int> pre (n + 1);
        pre[0] = 0;
        for (int i = 1; i <= n; i++) {
            pre[i] = hours[i-1] > 8 ? pre[i-1] + 1 : pre[i-1] - 1;
            if (pre[stk.top()] > pre[i]) {
                stk.push(i);
            }
        }
        int res = 0;
        for (int r = n; r >= 1; r--) {
            while (!stk.empty() && pre[stk.top()] < pre[r]) {
                res = max(res,r - stk.top());
                stk.pop();
            }
        }
        return res;
    }
};
```  
```cpp
class Solution {
public:
    int longestWPI(vector<int>& hours) {
        int n = hours.size();
        unordered_map<int, int> ump;
        int s = 0, res = 0;
        for (int i = 0; i < n; i++) {
            s += hours[i] > 8 ? 1 : -1;
            if (s > 0) {
                res = max(res, i + 1);
            } else {
                if (ump.count(s - 1)) {
                    res = max(res, i - ump[s - 1]);
                }
            }
            if (!ump.count(s)) {
                ump[s] = i;
            }
        }
        return res;
    }
};
```