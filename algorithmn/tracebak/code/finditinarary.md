题目描述：
![image](/algorithmn/tracebak/image/image13.png)  
解题过程：  
欧拉回路，欧拉通路，欧拉图，半欧拉图有向图和无向图的欧拉回路欧拉通路存在条件欧拉图半欧拉图存在条件，笑死，一个不会  
最大字典序，哈希表存储字符串到优先队列的映射关系，笑死又是一个不会  
题解强无敌只能说，真是太好理解了，若存在出度则选择字典序最大的路径然后删除该条路径进入下一个节点的查找，知道找到死胡同的时候才把当前节点加入栈中，则加入栈中的是可以寻找的路径上字典序最小的，最后翻转栈，OK  
move函数提高性能，学了一招  
代码：  
```cpp
class Solution {
private:
    unordered_map<string,priority_queue<string,vector<string>,greater<string>>> map;
    vector<string> stk;
public:
    void dfs(string curr) {
        while (map.count(curr) && map[curr].size() > 0) {
            string temp = map[curr].top();
            map[curr].pop();
            dfs(temp);
        }
        stk.push_back(curr);
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for (auto svec : tickets) {
            map[svec[0]].emplace(svec[1]);
        }
        dfs("JFK");
        reverse(stk.begin(),stk.end());
        return stk;
    }
};
```