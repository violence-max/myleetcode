题目描述：  
![image](/basicaldatastructure/linkedlist/image/image18.png)  
解题过程：  
很简单，但是还是写了二十分钟才用递归的方法写出来，不过效率比较低，每次递归都需要寻找第一个节点和最后一个节点，然后想用哈希表改进，虽然时间上大幅度进步但是还是不够，看了题解，发现每次操作时最后一个节点的前一个节点的next指针可以不用改变—因为下一次操作时它会改变的，因此只需要把最后一个节点的next指针置空就可以了。题解里还有个链表中点→反转链表→合并链表的方法，这里也写一下吧  
代码：（哈希表→链表中点、反转链表、合并链表）  
```cpp
class Solution {
private:
public:
    void reorderList(ListNode* head) {
        unordered_map<int,ListNode*> map;
        ListNode* tmp = head;
        int len = 0;
        while (tmp) {
            map[++len] = tmp;
            tmp = tmp->next;
        }
        int start = 1, end = len;
        while (end - start + 1 > 2) {
            map[start]->next = map[end];
            map[end]->next = map[start+1];
            start++;
            end--; 
        }
        map[end]->next = nullptr;
    }
};
```
```cpp
class Solution {
public:
    ListNode* middlenode(ListNode* head) {
        ListNode* slow = head, * fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    ListNode* reverselist(ListNode* head) {
        ListNode* pre = nullptr, * cur = head;
        while (cur) {
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }

    void merge(ListNode* l1,ListNode* l2) {
        while (l1 && l2) {
            ListNode* l1_tmp = l1->next, * l2_tmp = l2->next;
            l1->next = l2;
            l1 = l1_tmp;
            l2->next = l1;
            l2 = l2_tmp;
        }
    }

    void reorderList(ListNode* head) {
        ListNode* mid = middlenode(head);
        ListNode* l1 = head, * l2 = mid->next;
        // cout << l2->val;
        mid->next = nullptr;
        l2 = reverselist(l2);
        merge(l1,l2);
    }
};
```