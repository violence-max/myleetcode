题目描述：  
![image](/algorithmn/tracebak/image/image16.png)  
解题过程：  
一开始我还在想谁出的这么无聊的题目一点意义都没有，然后我把生成树给画了出来，我发现好像可以做？然后就用回溯法做出来了，但是一开始做的时候答案千奇百怪，原因是没有限定右括号数量小于左括号数量才加右括号，哎，还是肉眼debug才知道的  
代码：  
```cpp
class Solution {
private:
    vector<string> ans;
    string tmp;
public:
    void dfs(int n,int leftcount,int rightcount) {
        if (leftcount == n && rightcount == n) {
            ans.push_back(tmp);
            return;
        }
        if (leftcount < n) {
            tmp += '(';
            dfs(n,leftcount+1,rightcount);
            tmp.pop_back();
        }
        if (rightcount < n && rightcount < leftcount) {
            tmp += ')';
            dfs(n,leftcount,rightcount+1);
            tmp.pop_back();
        }
    }
    vector<string> generateParenthesis(int n) {
        dfs(n,0,0);
        return ans;
    }
};
```