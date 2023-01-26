题目描述：  
![image](/algorithmn/tracebak/image/image21.png)  
解题过程：  
这个题得好好记录以下算法了：  
字典树（为了练习字典树而写，跟写法没关系）  
1. 字典树的插入：首先建立一个node类，这个类里有个标志变量来标志node是否为最后一个节点，还有一个大小为26的装有指向node对象的指针的数组，一开始为空；每插入一个字符先计算index，如果对应位置上指针为空则新建一个节点，每次node都需要移动到对应位置（这部分简写就行）
2. 字典树的寻找：懒得写了，和插入一样
3. 回溯：（重中之重）这道题有关字典树的构建我就写了一下，但是有关回溯的算法花了将近一个小时才撸出来：
    1. 最终确定是通过循环的方式而不是每次回溯都添加一个字符的方式
    2. 每次对tmp添加一个字符，如果长度不够minlen则继续添加，够了并且小于等于maxlen就查字典，如果在字典里面而且已经dfs到s的最后一个字符了则往ans里加入tmp并且**删除最后一个单词**然后返回；不是s的最后一个字符就往tmp末尾加个空格然后继续dfs，dfs退出后把空格弹出；长度大于maxlen就直接跳出循环，**循环外面一定要弹出一个单词**  
代码：（字典树→记忆化搜索）  
```cpp
class Node {
private:
    bool isend;
public:
    vector<Node*> children;
    Node() {isend = false; children.resize(26,nullptr);}
    bool getend() {return isend;}
    void setend(bool _isend) {isend = _isend;}
};

class Trie {
private:
    Node* root;
    int minlen;
    int maxlen;
    string tmp;
public:
    Trie() {root = new Node();}

    void insert(const string& word) {
        Node* node = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word[i] - 'a';
            if (!node->children[index]) {
                node->children[index] = new Node();
            }
            node = node->children[index];
        }
        node->setend(true);
    }

    bool find(const string& word) {
        Node* node = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word[i] - 'a';
            if (!node->children[index]) {
                return false;
            }
            node = node->children[index];
        }
        return node->getend();
    }

    void setminlen(int minlen) {
        this->minlen = minlen;
    }

    void setmaxlen(int maxlen) {
        this->maxlen = maxlen;
    }

    void dfs(string& s, vector<string>& ans, int index, int start,int len) {
        for (int i = index; i < s.length(); i++) {
            tmp.push_back(s[i]);
            len++;
            if (len > maxlen) {
                break;
                return;
            } else if (len >= minlen && find(tmp.substr(start,len))) {
                if (i == s.length() - 1) {
                    ans.push_back(tmp);
                    tmp.erase(tmp.begin()+start,tmp.end());
                    return;
                } else {
                    tmp.push_back(' ');
                    dfs(s,ans,i+1,tmp.length(),0);
                    tmp.pop_back();
                }
            }
        }
        tmp.erase(tmp.begin()+start,tmp.end());
    }
};

class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        Trie t = Trie();
        int minlen = INT_MAX, maxlen = INT_MIN;
        for (auto& str : wordDict) {
            minlen = min(minlen,(int)str.length());
            maxlen = max(maxlen,(int)str.length());
            t.insert(str);
        }
        t.setminlen(minlen);t.setmaxlen(maxlen);
        vector<string> ans;
        t.dfs(s,ans,0,0,0);
        return ans;
    }
};
```
```cpp
class Solution {
private:
    unordered_map<int, vector<string>> ans;
    unordered_set<string> wordSet;

public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        wordSet = unordered_set(wordDict.begin(), wordDict.end());
        backtrack(s, 0);
        return ans[0];
    }

    void backtrack(const string& s, int index) {
        if (!ans.count(index)) {
            if (index == s.size()) {
                ans[index] = {""};
                return;
            }
            ans[index] = {};
            for (int i = index + 1; i <= s.size(); ++i) {
                string word = s.substr(index, i - index);
                if (wordSet.count(word)) {
                    backtrack(s, i);
                    for (const string& succ: ans[i]) {
                        ans[index].push_back(succ.empty() ? word : word + " " + succ);
                    }
                }
            }
        }
    }
};
```