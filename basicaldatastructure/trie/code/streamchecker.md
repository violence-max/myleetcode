题目描述：  
![image](/basicaldatastructure/trie/image/image3.png)  
解题过程：  
没做出来  
- 字典树（题解是AC自动机，我就不写了）
1. 每个字符串倒着插入
2. 维护已经在字典数里的字符串的最短长度，每次查询的时候将字符加入到字符串s中，如果s的长度小于最短长度可以直接退出
3. 每次search的时候只需要关注字符串s的后200位即可，当然，也是倒着查询
4. 每次查询如果当前节点为最后一个节点可以直接返回true，如果当前节点为空直接返回false  
代码：  
```cpp
class Trie {
private:
    struct TrieNode {
        bool isEnd;
        vector<TrieNode*> children;
        TrieNode() : isEnd(false), children(26, nullptr) {};
    };
    TrieNode* root;

public:
    Trie() : root(new TrieNode()) {}

    void insert(string& word) {
        TrieNode* node = root;
        int n = word.size();
        for (int i = n - 1; i >= 0; --i) {
            char c = word[i];
            c -= 'a';
            if (node->children[c] == nullptr) {
                node->children[c] = new TrieNode();
            }
            node = node->children[c];
        }
        node->isEnd = true;
        return;
    }

    bool search(string& word) {
        int n = word.size();
        TrieNode* node = root;
        for (int i = n - 1; i >= max(0, n - 200); --i) {
            char c = word[i];
            c -= 'a';
            if (node->isEnd || node->children[c] == nullptr) {
                return node->isEnd;
            }
            node = node->children[c];
        }
        return node->isEnd;
    }
};

class StreamChecker {
private:
    Trie* t;
    string s;
    int minLen;

public:
    StreamChecker(vector<string>& words) : t(new Trie()), s(""), minLen(2000) {
        for (string& s : words) {
            minLen = min(minLen, (int)s.size());
            t->insert(s);
        }
    }
    
    bool query(char letter) {
        s.push_back(letter);
        if (s.size() < minLen) {
            return false;
        }
        return t->search(s);
    }
};
```