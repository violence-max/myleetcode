题目描述：  
![image](/algorithmn/greed/image/image14.png)  
解题过程：  
这个题目也经历了好几个思想阶段：简单题，秒杀→发现在写右边界更新的判断条件的时候小于等于写成了大于等于→发现自己在定义答案二维矩阵的时候提前分配好了大小导致push的时候居然前四个值是控制→发现自己忘记处理最后一个边界的push了→发现自己忽略了在右边界升序排序的情况下有可能发生两个连续的右边界相同的区间第一个的左边界比第二个要大→发现根据右边界升序排序会发生最后一个范围是前面所有范围的集合的母体  
代码：  
```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),[](const vector<int>& u,const vector<int>& v){
            return u[0] < v[0] || (u[0] == v[0] && u[1] < v[1]);
        });
        int n = intervals.size();
        vector<vector<int>> ans;
        int right = intervals[0][1];
        int left = intervals[0][0];
        for (int i = 1; i < n; i++) {
            if (intervals[i][0] <= right) {
                right = (intervals[i][1] > right) ? intervals[i][1] : right;
            } else {
                vector<int> temp = {left,right};
                ans.push_back(temp);
                left = intervals[i][0];
                right = intervals[i][1];
            }
        }
        vector<int> temp = {left,right};
        ans.push_back(temp);
        return ans;
    }
};
```