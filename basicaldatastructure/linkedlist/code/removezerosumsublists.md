题目描述：

![image](/basicaldatastructure/linkedlist/image/image23.png)

解决过程：

做了30分钟左右，一开始想的是一次遍历，但是发现没办法递归，后面就改成两次遍历了，刚好这个数据量比较小可以过。看了题解，还是哈希的解法比较好一点。

代码：（两次便利→前缀和+哈希表）

```cpp
class Solution {
public:
    ListNode* removeZeroSumSublists(ListNode* head) {
        if (!head) {
            return nullptr;
        }

        ListNode* pre = head, *cur = head->next;
        int sum = head->val;
        if (sum == 0) {
            return removeZeroSumSublists(head->next);
        }
        while (cur) {
            while (cur && sum + cur->val != 0) {
                sum += cur->val;
                cur = cur->next;
            }
            
            if (!cur) {
                pre->next = removeZeroSumSublists(pre->next);
            } else {
                return removeZeroSumSublists(cur->next);
            }
        }
        return head;
    }
};
```

```cpp
class Solution {
public:
    ListNode* removeZeroSumSublists(ListNode* head) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        int prefix = 0;
        unordered_map<int, ListNode*> seen;
        for (ListNode* node = dummy; node; node = node->next) {
            prefix += node->val;
            seen[prefix] = node;
        }
        prefix = 0;
        for (ListNode* node = dummy; node; node = node->next) {
            prefix += node->val;
            node->next = seen[prefix]->next;
        }
        return dummy->next;
    }
};
```