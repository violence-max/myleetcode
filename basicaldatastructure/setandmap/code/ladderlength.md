题目描述：  
![image](/basicaldatastructure/setandmap/image/image3.png)  
写了II，再来写I，5分钟ac。关键在于不需要from来连接节点，不需要steps来保护可靠路径，找到了就返回，很爽  
题解提供了虚拟节点的方法，很有趣我觉得，hot节点，将第一个字符修改成’*’变成*ot，这样，当dot也修改第一个字符时，已经存在的虚拟节点会和dot连成一条双向的边，再使用wordid哈希表进行辅助，也是可以做出来的，而且效率我认为并不低。还有双向bfs来减少时间的，虽然这跟测试用例有很大关系  
代码：（经典BFS→虚拟节点bfs→双向bfs）  
```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict (wordList.begin(),wordList.end());        
        if (dict.find(endWord) == dict.end()) {
            return 0;
        }
        dict.erase(beginWord);
        int step = 1;
        queue<string> q;
        q.push(beginWord);
        int length = beginWord.length();
        bool found = false;
        while (!q.empty()) {
            int size = q.size();
            step++;
            for (int i = 0; i < size; i++) {
                const string currword = move(q.front());
                string nextword = currword;
                q.pop();
                for (int j = 0; j < length; j++) {
                    const char origin = nextword[j];
                    for (char x = 'a'; x <= 'z'; x++) {
                        nextword[j] = x;
                        if (dict.find(nextword) == dict.end()) {
                            continue;
                        }
                        dict.erase(nextword);
                        q.push(nextword);
                        if (nextword == endWord) {
                            found = true;
                        }
                    }
                    nextword[j] = origin;
                }
                if (found) return step;
            }
        }
        return 0;
    }
};
```
```cpp
class Solution {
public:
    unordered_map<string, int> wordId;
    vector<vector<int>> edge;
    int nodeNum = 0;

    void addWord(string& word) {
        if (!wordId.count(word)) {
            wordId[word] = nodeNum++;
            edge.emplace_back();
        }
    }

    void addEdge(string& word) {
        addWord(word);
        int id1 = wordId[word];
        for (char& it : word) {
            char tmp = it;
            it = '*';
            addWord(word);
            int id2 = wordId[word];
            edge[id1].push_back(id2);
            edge[id2].push_back(id1);
            it = tmp;
        }
    }

    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        for (string& word : wordList) {
            addEdge(word);
        }
        addEdge(beginWord);
        if (!wordId.count(endWord)) {
            return 0;
        }
        vector<int> dis(nodeNum, INT_MAX);
        int beginId = wordId[beginWord], endId = wordId[endWord];
        dis[beginId] = 0;

        queue<int> que;
        que.push(beginId);
        while (!que.empty()) {
            int x = que.front();
            que.pop();
            if (x == endId) {
                return dis[endId] / 2 + 1;
            }
            for (int& it : edge[x]) {
                if (dis[it] == INT_MAX) {
                    dis[it] = dis[x] + 1;
                    que.push(it);
                }
            }
        }
        return 0;
    }
};
```
```cpp
class Solution {
public:
    unordered_map<string, int> wordId;
    vector<vector<int>> edge;
    int nodeNum = 0;

    void addWord(string& word) {
        if (!wordId.count(word)) {
            wordId[word] = nodeNum++;
            edge.emplace_back();
        }
    }

    void addEdge(string& word) {
        addWord(word);
        int id1 = wordId[word];
        for (char& it : word) {
            char tmp = it;
            it = '*';
            addWord(word);
            int id2 = wordId[word];
            edge[id1].push_back(id2);
            edge[id2].push_back(id1);
            it = tmp;
        }
    }

    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        for (string& word : wordList) {
            addEdge(word);
        }
        addEdge(beginWord);
        if (!wordId.count(endWord)) {
            return 0;
        }

        vector<int> disBegin(nodeNum, INT_MAX);
        int beginId = wordId[beginWord];
        disBegin[beginId] = 0;
        queue<int> queBegin;
        queBegin.push(beginId);

        vector<int> disEnd(nodeNum, INT_MAX);
        int endId = wordId[endWord];
        disEnd[endId] = 0;
        queue<int> queEnd;
        queEnd.push(endId);

        while (!queBegin.empty() && !queEnd.empty()) {
            int queBeginSize = queBegin.size();
            for (int i = 0; i < queBeginSize; ++i) {
                int nodeBegin = queBegin.front();
                queBegin.pop();
                if (disEnd[nodeBegin] != INT_MAX) {
                    return (disBegin[nodeBegin] + disEnd[nodeBegin]) / 2 + 1;
                }
                for (int& it : edge[nodeBegin]) {
                    if (disBegin[it] == INT_MAX) {
                        disBegin[it] = disBegin[nodeBegin] + 1;
                        queBegin.push(it);
                    }
                }
            }

            int queEndSize = queEnd.size();
            for (int i = 0; i < queEndSize; ++i) {
                int nodeEnd = queEnd.front();
                queEnd.pop();
                if (disBegin[nodeEnd] != INT_MAX) {
                    return (disBegin[nodeEnd] + disEnd[nodeEnd]) / 2 + 1;
                }
                for (int& it : edge[nodeEnd]) {
                    if (disEnd[it] == INT_MAX) {
                        disEnd[it] = disEnd[nodeEnd] + 1;
                        queEnd.push(it);
                    }
                }
            }
        }
        return 0;
    }
};
```