题目描述：  
![image](/basicaldatastructure/stackandquere/image/image19.png)  
解题过程：  
第一反应暴力肯定不行，因为可能性太多了  
看了题解，从数据结构挑选最优班级这个思路太好了。叠加优先队列和糖水原理就可以题解  
要点：  
$\frac{pass+2}{total+2} - \frac{pass+1}{total+1} < \frac{pass+1}{total+1} - \frac{pass}{total}$ ti  
代码:  
```cpp
class Solution {
public:
    struct Ratio {
        int pass, total;
        bool operator < (const Ratio& oth) const {
            return (long long)(oth.total + 1) * (oth.total) * (total - pass) < 
                   (long long)(total + 1) * (total) * (oth.total - oth.pass);
        }
    };

    double maxAverageRatio(vector<vector<int>>& classes, int extraStudents) {
        priority_queue<Ratio> q;
        for (auto& c : classes) {
            q.push({c[0],c[1]});
        }
        for (int i = 0; i < extraStudents; i++) {
            auto [pass,total] = q.top();
            q.pop();
            q.push({pass + 1,total + 1});
        }
        double res = 0;
        for (int i = 0; i < classes.size(); i++) {
            auto [pass,total] = q.top();
            q.pop();
            res += 1.0 * pass / total;
        }
        return res / classes.size();
    }
};
```