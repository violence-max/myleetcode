题目描述:  
![image](/algorithmn/tracebak/image/image23.png)  
解决过程：  
又是自己没有想出来  
今天官方偷懒了，没有写题解，看了一个code友的解答。没想到居然用暴力（回溯+记忆化搜索做出来了）。一开始我想的还是后缀和数组来着，现在看来前缀和数组就好了  
逻辑在代码上写得很清晰  
代码:  
```cpp
class Solution {
private:
    vector<int> pre;
    vector<vector<int>> mem;
    int n;
public:
    int dfs(int index, int M) {
        if (2 * M >= n - index) {
            return pre[n] - pre[index];
        } else if (mem[index][M]) return mem[index][M];
        else {
            int ret = INT_MIN;
            for (int x = 1; x <= 2 * M; x++) {
                ret = max(ret,pre[n] - pre[index] - dfs(index + x,max(M,x)));
            }
            return mem[index][M] = ret;
        }
    }

    int stoneGameII(vector<int>& piles) {
        n = piles.size();
        pre.resize(n + 1);
        pre[0] = 0;
        for (int i = 0; i < n; i++) {
            pre[i + 1] = pre[i] + piles[i];
        }
        mem.resize(n + 1,vector<int> (n + 1));
        return dfs(0,1);
    }
};
```