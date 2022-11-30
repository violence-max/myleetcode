题目描述：  
![image](/algorithmn/greed/image/image1.png)  
解题过程：  
简单是简单，但是居然还写了至少十分钟感觉，而且循环的时候变量又没有写明白，我真无敌。  
代码：  
```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        if (s.size() == 0) return 0;
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        queue<int> q;
        for (int i = 0; i < s.size(); i++) {
            q.push(s[i]);
        }
        int ret = 0;
        for (int i = 0; i < g.size(); i++) {
            while (!q.empty()) {
                int min = q.front();
                if (min >= g[i]) {
                    ret++;
                    q.pop();
                    break;
                }
                q.pop();
            }
        }
        return ret;
    }
};
```