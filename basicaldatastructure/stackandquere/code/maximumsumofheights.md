题目描述：

![image](/basicaldatastructure/stackandquere/image/image32.png)

解决过程：

第364场周赛t3，没做出来

- 前后缀分解+单调栈

代码：

```cpp
class Solution {
public:
    long long maximumSumOfHeights(vector<int>& maxHeights) {
        int n = maxHeights.size();
        long long pre[n+1], suf[n+1];
        memset(pre, 0, sizeof pre);
        memset(suf, 0, sizeof suf);
        stack<int> stk;
        stk.push(n);
        long long sum = 0;
        for (int i = n - 1; i >= 0; i--) {
            int x = maxHeights[i];
            while (stk.size() > 1 && x <= maxHeights[stk.top()]) {
                int j = stk.top();
                stk.pop();
                sum -= (long long)maxHeights[j] * (stk.top() - j);
            }
            sum += (long long)x * (stk.top() - i);
            stk.push(i);
            suf[i] = sum;
        }
        long long ans = INT_MIN, presum = 0;
        stack<int> st;
        st.push(-1);
        for (int i = 0; i < n; ++i) {
            int x = maxHeights[i];
            while (st.size() > 1 && x <= maxHeights[st.top()]) {
                int j = st.top();
                st.pop();
                presum -= (long long)maxHeights[j] * (j - st.top());
            }
            presum += (long long)x * (i - st.top());
            st.push(i);
            pre[i] = presum;
            ans = max(ans, pre[i] + suf[i+1]);
        }
        return ans;
    }
};
```