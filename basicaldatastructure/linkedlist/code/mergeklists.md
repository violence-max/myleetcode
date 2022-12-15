题目描述：  
![image](/basicaldatastructure/linkedlist/image/image9.png)  
解题过程：  
操了，这么简单的解题思路，居然还花了我二十分钟的gdb时间，主要是以为一个节点初始化会默认为空指针，没有想到啊居然不是，必须显式地将其赋值为空指针才可以  
看了题解，分支的思路时间复杂度低了一个级别，很香  
代码：  
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* merge(ListNode* root,ListNode* another) {
        if (!root) return another;
        ListNode* tmp = new ListNode(0);
        ListNode* head = tmp;
        while (root && another) {
            if (root->val < another->val) {
                tmp->next = root;
                root = root->next;
            } else {
                tmp->next = another;
                another = another->next;
            }
            tmp = tmp->next;
        }
        tmp->next = (root) ? root : another;
        return head->next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int n = lists.size();
        if (n == 0) return nullptr;
        ListNode* ans = lists[0];
        for (int i = 1; i < n; i++) {
            ans = merge(ans,lists[i]);
        }
        return ans;
    }
};
```  
分治法：  
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
        if ((!a) || (!b)) return a ? a : b;
        ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
        while (aPtr && bPtr) {
            if (aPtr->val < bPtr->val) {
                tail->next = aPtr; aPtr = aPtr->next;
            } else {
                tail->next = bPtr; bPtr = bPtr->next;
            }
            tail = tail->next;
        }
        tail->next = (aPtr ? aPtr : bPtr);
        return head.next;
    }

    ListNode* merge(vector <ListNode*> &lists, int l, int r) {
        if (l == r) return lists[l];
        if (l > r) return nullptr;
        int mid = (l + r) >> 1;
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        return merge(lists, 0, lists.size() - 1);
    }
};
```