题目描述：  
![image](/basical/gragh/image/image4.png)  
解题过程：  
今天了解了一下acm爷，发现leetcode只是小儿科，然而我还是那么菜，心态被搞了，bfs写了半天。  
代码：（bfs→dfs）  
```cpp
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (!node) return nullptr;
        queue<Node*> q, cq;
        Node* root = new Node(node->val);
        q.push(root); cq.push(node);
        unordered_map<int,Node*> map;
        map[root->val] = root;
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                Node* tmp = q.front(); q.pop();
                Node* origin = cq.front(); cq.pop();
                for (auto& neighbor : origin->neighbors) {
                    if (!map.count(neighbor->val)) {
                        Node* newnode = new Node(neighbor->val);
                        tmp->neighbors.push_back(newnode);
                        q.push(newnode);
                        map[newnode->val] = newnode;
                        cq.push(neighbor);
                    } else {
                        tmp->neighbors.push_back(map[neighbor->val]);
                    }
                }
            }
        }
        return root;
    }
};
```
```cpp
class Solution {
private:
    unordered_map<int,Node*> map;
public:
    Node* dfs(Node* node,Node* origin) {
        if (map.count(origin->val)) {
            node->neighbors.push_back(map[origin->val]);
            return nullptr;
        } else {
            Node* newnode = new Node(origin->val);
            map[newnode->val] = newnode;
            if (node) node->neighbors.push_back(newnode);
            for (auto& neighbor : origin->neighbors) {
                dfs(newnode,neighbor);
            }
            return newnode;
        }
    }
    Node* cloneGraph(Node* node) {
        if (!node) return nullptr;
        return dfs(nullptr,node);
    }
};
```