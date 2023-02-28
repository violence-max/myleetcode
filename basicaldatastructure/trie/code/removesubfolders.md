题目描述：  
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bb04b0f9-9cc6-468c-a3bb-e482b04df4ad/Untitled.png)  
解决过程：  
写了一个字典树，但是在实现字典树的节点的时候犯了糊涂，不知道怎么样从一个头节点指向拥有各个前缀的节点，最终得到的答案是实现一个字符串到指向节点的指针的一个映射。还有我一开始是没有想到要排序的，如果使用字典树那么必须需要的是先插入父，再插入孩子的时候才能检测出该文件夹是不是子文件夹  
题解给了一些很不错的做法，这里也记录一下  
代码：（字典树+排序→判断前缀+排序→字典树+回溯）  
```cpp
class Node {
public:
    Node() : is_end(false) {;}
    unordered_map<string, Node*> set;
    bool is_end;
};

class Trie {
private:
    Node* root;
public:
    Trie(): root (new Node()) {;}

    bool insert(string path) {
        int len = path.length();
        Node* _root = root;
        for (int i = 1; i < len; i++) {
            int start = i;
            while (i < len && path[i] != '/') i++;
            string sub = path.substr(start,i - start);
            if (!_root->set.count(sub)) {
                _root->set[sub] = new Node();
            }
            _root = _root->set[sub];
            if (_root->is_end) return false;
        }
        _root->is_end = true; 
        return true;
    }
};

class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        vector<string> ans;
        int n = folder.size();
        Trie trie = Trie();
        sort(folder.begin(),folder.end());
        for (int i = 0; i < n; i++) {
            if (trie.insert(folder[i])) {
                ans.emplace_back(folder[i]);
            }
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        sort(folder.begin(), folder.end());
        vector<string> ans = {folder[0]};
        for (int i = 1; i < folder.size(); ++i) {
            if (int pre = ans.end()[-1].size(); !(pre < folder[i].size() && ans.end()[-1] == folder[i].substr(0, pre) && folder[i][pre] == '/')) {
                ans.push_back(folder[i]);
            }
        }
        return ans;
    }
};
```  
```cpp
struct Trie {
    Trie(): ref(-1) {}

    unordered_map<string, Trie*> children;
    int ref;
};

class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        auto split = [](const string& s) -> vector<string> {
            vector<string> ret;
            string cur;
            for (char ch: s) {
                if (ch == '/') {
                    ret.push_back(move(cur));
                    cur.clear();
                }
                else {
                    cur.push_back(ch);
                }
            }
            ret.push_back(move(cur));
            return ret;
        };

        Trie* root = new Trie();
        for (int i = 0; i < folder.size(); ++i) {
            vector<string> path = split(folder[i]);
            Trie* cur = root;
            for (const string& name: path) {
                if (!cur->children.count(name)) {
                    cur->children[name] = new Trie();
                }
                cur = cur->children[name];
            }
            cur->ref = i;
        }

        vector<string> ans;

        function<void(Trie*)> dfs = [&](Trie* cur) {
            if (cur->ref != -1) {
                ans.push_back(folder[cur->ref]);
                return;
            }
            for (auto&& [_, child]: cur->children) {
                dfs(child);
            }
        };

        dfs(root);
        return ans;
    }
};
```