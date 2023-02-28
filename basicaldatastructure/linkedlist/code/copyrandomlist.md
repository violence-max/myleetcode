题目描述：  
![image](/basicaldatastructure/linkedlist/image/image16.png)  
解题过程：  
很简单，我一个深度优先搜索就写出来了，看了题解，写得比我简洁很多，原因是哈希表和深度优先用得比我好。还有先修改原链表然后还原得出结果的方法，具体看代码  
代码：（dfs→官解dfs→修改原链表）  
```cpp
class Solution {
private:
    unordered_map<Node*,Node*> map;
public:
    Node* copyRandomList(Node* head) {
        if (!head) return nullptr;
        Node* node;
        if (!map.count(head)) {
            node = new Node(head->val);
            map[head] = node;
        } else {
            node = map[head];
        }
        if (head->random) {
            Node* r;
            if (!map.count(head->random)) {
                r = new Node(head->random->val);
                map[head->random] = r;
            } else {
                r = map[head->random];
            }
            node->random = r;
        }
        node->next = copyRandomList(head->next);
        return node;
    }
};
```
```cpp
class Solution {
public:
    unordered_map<Node*, Node*> cachedNode;

    Node* copyRandomList(Node* head) {
        if (head == nullptr) {
            return nullptr;
        }
        if (!cachedNode.count(head)) {
            Node* headNew = new Node(head->val);
            cachedNode[head] = headNew;
            headNew->next = copyRandomList(head->next);
            headNew->random = copyRandomList(head->random);
        }
        return cachedNode[head];
    }
};
```
```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == nullptr) {
            return nullptr;
        }
        for (Node* node = head; node != nullptr; node = node->next->next) {
            Node* nodeNew = new Node(node->val);
            nodeNew->next = node->next;
            node->next = nodeNew;
        }
        for (Node* node = head; node != nullptr; node = node->next->next) {
            Node* nodeNew = node->next;
            nodeNew->random = (node->random != nullptr) ? node->random->next : nullptr;
        }
        Node* headNew = head->next;
        for (Node* node = head; node != nullptr; node = node->next) {
            Node* nodeNew = node->next;
            node->next = node->next->next;
            nodeNew->next = (nodeNew->next != nullptr) ? nodeNew->next->next : nullptr;
        }
        return headNew;
    }
};
```