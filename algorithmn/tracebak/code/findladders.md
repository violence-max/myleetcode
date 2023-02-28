题目描述：  
![image](/algorithmn/tracebak/image/image20.png)  
解决过程：  
这道题是真的难，BFS加回溯，难得批爆。  
关键思路在于将其转换为图，只相差一个字母的单词可以互相连接然后把问题转化成单源最短路径。细节是真的多：哈希表的快速构造、特殊用例的处理（字典里不存在endword和存在beginword）、初始化图表时beginword指向的是空的哈希集合、广度优先搜索的按层次与单独弹出、在单词的基础上遍历每个位置的每个字母而不是查找字典再判断是否对应、在字典里找到元素之后删除元素防止复杂情况的出现、通过记录每个单词的层数来处理在字典里已经被删除的元素但是仍然有单词可以到达这个元素的情况、使用found标志变量来提前退出广度优先搜索并且以此为依据来判断是否有可靠路径、反向迭代器配合push_back()来使用来将一块连续空间的迭代器加入到数组中  
代码：  
```cpp
class Solution {
public:
    void dfs(vector<vector<string>>& res,const string& node,unordered_map<string,unordered_set<string>>& from,
    vector<string>& path) {
        if (from[node].empty()) {
            res.push_back({path.rbegin(),path.rend()});
            return;
        }
        for (const string& p : from[node]) {
            path.push_back(p);
            dfs(res,p,from,path);
            path.pop_back();
        }
    }

    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        vector<vector<string>> res;
        unordered_set<string> dict (wordList.begin(),wordList.end());
        if (dict.find(endWord) == dict.end()) {
            return res;
        }
        dict.erase(beginWord);
        int length = beginWord.length();
        queue<string> q;
        q.push(beginWord);
        int step = 0;
        bool found = false;
        unordered_map<string,unordered_set<string>> from = {{beginWord,{}}};
        unordered_map<string,int> steps = {{beginWord,0}};
        while (!q.empty()) {
            int size = q.size();
            step++;
            for (int i = 0; i < size; i++) {
                const string currword = move(q.front());
                string nextword = currword;
                q.pop();
                for (int k = 0; k < length; k++) {
                    const char origin = nextword[k];
                    for (char j = 'a'; j <= 'z'; j++) {
                        nextword[k] = j;
                        if (steps[nextword] == step) {
                            from[nextword].insert(currword);
                        }
                        if (dict.find(nextword) == dict.end()) {
                            continue;
                        }
                        dict.erase(nextword);
                        q.push(nextword);
                        steps[nextword] = step;
                        from[nextword].insert(currword);
                        if (nextword == endWord) {
                            found = true;
                        }
                    }
                    nextword[k] = origin;
                }
            }
            if (found) {
                break;
            }
        }
        if (found) {
            vector<string> path = {endWord};
            dfs(res,endWord,from,path);
        }
        return res;
    }
};
```