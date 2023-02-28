题目描述：  
![image](/basicaldatastructure/trie//image/image1.png)  
题目描述：  
写得很快。题解写得也不错，记录一下  
代码：（我的→题解）  
```cpp
class Node {
private:
    bool is_end;
public :
    vector<Node*> arr;
    Node() {arr.resize(26); is_end = false;}
    void setEnd(bool is_end) {this->is_end = is_end;}
    bool getEnd() {return is_end;}
};

class Trie {
private:
    Node* root;
public:
    Trie() {
        root = new Node();
    }
    
    void insert(string word) {
        Node* _root = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word[i] - 'a';
            if (!_root->arr[index]) {
                _root->arr[index] = new Node();
            }
            _root = _root->arr[index];
            if (i == word.length() - 1) _root->setEnd(true);
        }
    }
    
    bool search(string word) {
        Node* _root = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word[i] - 'a';
            if (!_root->arr[index]) {
                return false;
            }
            _root = _root->arr[index];
        }
        return _root->getEnd();
    }
    
    bool startsWith(string prefix) {
        Node* _root = root;
        for (int i = 0; i < prefix.length(); i++) {
            int index = prefix[i] - 'a';
            if (!_root->arr[index]) {
                return false;
            }
            _root = _root->arr[index];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```
```cpp
class Trie {
private:
    vector<Trie*> children;
    bool isEnd;

    Trie* searchPrefix(string prefix) {
        Trie* node = this;
        for (char ch : prefix) {
            ch -= 'a';
            if (node->children[ch] == nullptr) {
                return nullptr;
            }
            node = node->children[ch];
        }
        return node;
    }

public:
    Trie() : children(26), isEnd(false) {}

    void insert(string word) {
        Trie* node = this;
        for (char ch : word) {
            ch -= 'a';
            if (node->children[ch] == nullptr) {
                node->children[ch] = new Trie();
            }
            node = node->children[ch];
        }
        node->isEnd = true;
    }

    bool search(string word) {
        Trie* node = this->searchPrefix(word);
        return node != nullptr && node->isEnd;
    }

    bool startsWith(string prefix) {
        return this->searchPrefix(prefix) != nullptr;
    }
};
```