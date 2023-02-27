题目描述：  
![image](/basicaldatastructure/binary_tree/image/image51.png)  
解题过程：  
其实我是知道这个题目是什么意思的，但是这种题目套路的模板实在是太明显了，就没有去细想了  
看了题解，两种方法：  
1. 先序遍历：使用双向链表这个数据结构能够优化数组带来的删除头节点所消耗的时间复杂度
2. 括号编码：用到了编译原理的LL(1)文法，我没有细看，感觉稍微有点复杂  
代码:（先序遍历→括号编码+下降解码）  
```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root) return "None,";
        string res =  to_string(root->val) + ',' + serialize(root->left) + serialize(root->right);
        return res;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        list<string> _list;
        string str;
        for (auto& ch : data) {
            if (ch == ',') {
                _list.emplace_back(str);
                str.clear();
            } else {
                str.push_back(ch);
            }
        }
        if (!str.empty()) {
            _list.emplace_back(str);
            str.clear();
        }
        return dfs(_list);
    }

    TreeNode* dfs(list<string>& data) {
        if (data.front() == "None") {
            data.erase(data.begin());
            return nullptr;
        }

        TreeNode* _root = new TreeNode(stoi(data.front(),0,10));
        data.erase(data.begin());
        _root->left = dfs(data);
        _root->right = dfs(data);
        return _root;
    }
};
```  
```cpp
class Codec {
public:
    string serialize(TreeNode* root) {
        if (!root) {
            return "X";
        }
        auto left = "(" + serialize(root->left) + ")";
        auto right = "(" + serialize(root->right) + ")";
        return left + to_string(root->val) + right;
    }

    inline TreeNode* parseSubtree(const string &data, int &ptr) {
        ++ptr; // 跳过左括号
        auto subtree = parse(data, ptr);
        ++ptr; // 跳过右括号
        return subtree;
    }

    inline int parseInt(const string &data, int &ptr) {
        int x = 0, sgn = 1;
        if (!isdigit(data[ptr])) {
            sgn = -1;
            ++ptr;
        }
        while (isdigit(data[ptr])) {
            x = x * 10 + data[ptr++] - '0';
        }
        return x * sgn;
    }

    TreeNode* parse(const string &data, int &ptr) {
        if (data[ptr] == 'X') {
            ++ptr;
            return nullptr;
        }
        auto cur = new TreeNode(0);
        cur->left = parseSubtree(data, ptr);
        cur->val = parseInt(data, ptr);
        cur->right = parseSubtree(data, ptr);
        return cur;
    }

    TreeNode* deserialize(string data) {
        int ptr = 0;
        return parse(data, ptr);
    }
};
```