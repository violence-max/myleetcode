题目描述：  
![image](/basical/array/image/image28.png) 
我认为区间问题应该成为数组部分一个重点内容，属于是玩弄数组下标的类型。  
这个题是看题解做出来的，笑死，根本没有想到不断更改插入区间的左右端点的值。  
没有看题解的代码，仅仅是看了思路之后就去自己尝试了，结果捣鼓了半个多小时才敲出来。第一遍扫描原区间数组，不断更改插入区间的左右端点的值并且向答案中加入与插入区间不相交的区间。第二遍扫描答案数组，为插入区间寻找一个合适的插入位置：  
1. 若答案数组为空则直接加入插入区间
2. 否则，若插入区间位于答案数组的两头，则插入
3. 插入区间位于答案数组的中间，找到一个合适的位置插入  
写插入部分花的时间最久，原因是对insert函数不是很熟悉。insert函数的两个参数，第一个参数是迭代器，第二个参数是待插入的值。insert(it.begin(),x)，表示在容器it的第0个位置插入x，按1进行计数，这样就可以做到头插入或者尾插入了。  
代码：（自己写的） 
```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> ans;
        for (auto v : intervals) {
            if (v[1] < newInterval[0] || v[0] > newInterval[1]) ans.push_back(v);
            else {
                newInterval[0] = min(newInterval[0],v[0]);
                newInterval[1] = max(newInterval[1],v[1]);
            }
        }
        if (ans.empty()) ans.push_back(newInterval);
        else {
            int m = ans.size();
            if (newInterval[1] < ans[0][0]) ans.insert(ans.begin(),newInterval);
            else if (newInterval[0] > ans[m-1][1]) ans.insert(ans.begin()+m,newInterval);
            else {
                for (int i = 1; i < m; i++) {
                    if (newInterval[1] < ans[i][0] && newInterval[0] > ans[i-1][1]) {
                        ans.insert(ans.begin()+i,newInterval);
                        break;
                    }
                }
            }
        }
        return ans;
    }
};
```  
题解：  
这部分相当精髓，直接在遍历原区间数组的时候进行了插入区间的加入，原因就是更新了插入区间之后新的插入区间不可能跟上一个遍历到的区间有重合，所以可以只通过判断下一个区间的左端点是否大于插入区间的右端点即可，加上标志变量，强无敌。  
```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int left = newInterval[0];
        int right = newInterval[1];
        bool placed = false;
        vector<vector<int>> ans;
        for (const auto& interval: intervals) {
            if (interval[0] > right) {
                if (!placed) {
                    ans.push_back({left, right});
                    placed = true;                    
                }
                ans.push_back(interval);
            }
            else if (interval[1] < left) {
                ans.push_back(interval);
            }
            else {
                left = min(left, interval[0]);
                right = max(right, interval[1]);
            }
        }
        if (!placed) {
            ans.push_back({left, right});
        }
        return ans;
    }
};
```