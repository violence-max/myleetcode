题目描述：

![image](/basicaldatastructure/linkedlist/image/image25.png)

解决过程：

第358场周赛t2，ac

这里用了递归的做法，空间复杂度On，mark一下空间复杂度O1的做法：遇到val大于4的下一个节点则加1

代码：（递归→O1）

```cpp
class Solution {
public:
    ListNode* doubleIt(ListNode* head) {
        function<int(ListNode*, int)> dfs = [&](ListNode* cur, int c) -> int {
            if (cur->next) {
                c = dfs(cur->next, c);
            }
            
            int val = cur->val;
            int x = val * 2 + c;
            int nv = x % 10, nc = x / 10;
            cur->val = nv;
            return nc;
        };
        int c = dfs(head, 0);
        if (c > 0) {
            ListNode* new_head = new ListNode(1, head);
            return new_head;
        } else return head;
    }
};
```

```cpp
class Solution {
public:
    ListNode *doubleIt(ListNode *head) {
        if (head->val > 4)
            head = new ListNode(0, head);
        for (auto cur = head; cur; cur = cur->next) {
            cur->val = cur->val * 2 % 10;
            if (cur->next && cur->next->val > 4)
                cur->val++;
        }
        return head;
    }
};
```