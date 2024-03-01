题目描述：

![image](/basicaldatastructure/binary_tree/image/image66.png)

解决过程：

没做出来

- 根据二叉搜索树的后序遍历结果可以还原二叉搜索树

代码：

```cpp
class Codec {
public:

    void postorder(TreeNode* root, vector<int>& arr) {
        if (!root) return ;

        postorder(root->left, arr);
        postorder(root->right, arr);
        arr.push_back(root->val);
    }

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        vector<int> arr;
        postorder(root, arr);
        if (arr.empty()) return "";
        string ret;
        int n = arr.size();
        for (int i = 0; i < n - 1; ++i) {
            ret += to_string(arr[i]);
            ret += ',';
        }
        ret += to_string(arr[n-1]);
        // cout << ret << endl;
        return ret;
    }

    void split(const string& data, char dec, vector<int>& arr) {
        int n = data.size();
        int pos = 0;
        while (pos < n) {
            if (pos < n && data[pos] == dec) {
                ++pos;
            }
            int start = pos;
            while (pos < n && data[pos] != dec) {
                ++pos;
            }
            arr.push_back(stoi(data.substr(start, pos - start)));
        }
    }

    TreeNode* construct(int lower, int upper, vector<int>& arr) {
        if (arr.empty() || arr.back() < lower || arr.back() > upper) return nullptr;

        int val = arr.back();
        arr.pop_back();
        TreeNode* newNode = new TreeNode(val);
        // cout << val << endl;
        newNode->right = construct(val, upper, arr);
        // if (newNode->right) cout << newNode->right->val << endl;
        newNode->left = construct(lower, val, arr);
        // if (newNode->left) cout << newNode->left->val << endl;
        return newNode;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data.size() == 0) return nullptr;

        vector<int> arr;
        split(data, ',', arr);
        // for (int i : arr) cout << i << ' ';
        // cout << endl;
        TreeNode* root = construct(INT_MIN, INT_MAX, arr);
        // cout << root->val << endl;
        return root;
    }
};
```