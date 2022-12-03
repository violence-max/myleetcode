题目描述：  
![image](/algorithmn/greed/image/image10.png)  
解题过程：  
这道题是真的不会，觉得一个二维矩阵，要处理第一个通道又要处理第二个通道实在是没有办法。  
题解真的太寂寞了。直接按身高升序排序按人数降序排序，先排矮人再排高人，而且选空插入和排序函数的调用真的是寂寞的不行，牛逼。  
代码：  
```cpp
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),[](const vector<int> &u,const vector<int> &v){
            return u[0] < v[0] || (u[0] == v[0] && u[1] > v[1]);
        });
        int n = people.size();
        vector<vector<int>> ans (n);
        for (const vector<int> &person : people) {
            int spaces = person[1] + 1;
            for (int i = 0; i < n; i++) {
                if (ans[i].empty()) {
                    spaces--;
                    if (spaces == 0) {
                        ans[i] = person;
                    }
                }
            }
        }
        return ans;
    }
};
```