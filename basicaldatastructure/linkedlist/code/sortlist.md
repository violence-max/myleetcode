题目描述：  
![image](/basicaldatastructure/linkedlist/image/image19.png)  
解题过程：  
这个题不能直接使用插入排序，需要nlogn的时间复杂度  
我用的是哈希表记录值到节点的映射，还是插入排序，只不过使用set进行了辅助查找插入位置，还是coding了一段时间的，主要是不知道upper_bound是查找第一个值比target大的迭代器，而且对这个api也不太熟悉  
题解里自顶向下归并排序很好写（虽然求链表中点还是不太熟），但是自底向上归并排序就没那么好写了，主要卡在了思考要进行几次自底向上的归并排序上，最后发现如果要归并的子序列的长度是一定会大于等于链表长度的二分之一的，然后在下一次循环时这个长度就会大于链表的长度最终退出循环，因此for循环的判断条件直接写小于len是没有问题的；还有一个问题就是每次局部归并结束以后需要把prev指针移动到尾部，这里我采取的是使用tail指针在merge函数里面动手脚，题解是直接移动prev，我认为更加简洁  
代码：（set→自顶向下归并排序→自底向上归并排序）  
```cpp
class Solution {
private:
    unordered_map<int,ListNode*> map;
    multiset<int> s;
public:
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode* dummyhead = new ListNode(INT_MIN,head);
        ListNode* lastsort = dummyhead->next;
        map[lastsort->val] = lastsort;
        map[INT_MIN] = dummyhead;
        s.insert(lastsort->val);
        s.insert(INT_MIN);
        ListNode* cur = lastsort->next;
        while (cur) {
            if (cur->val >= lastsort->val) {
                lastsort = lastsort->next;
                map[lastsort->val] = lastsort;
                s.insert(lastsort->val);
            } else {
                lastsort->next = cur->next;
                ListNode* prev = map[*(--s.upper_bound(cur->val))];
                cur->next = prev->next;
                prev->next = cur;
                s.insert(cur->val);
                map[cur->val] = cur;
            }
            cur = lastsort->next;
        }
        return dummyhead->next;
    }
};
```
```cpp
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode* dummyhead = new ListNode(0);
        ListNode* cur = dummyhead;
        while (l1 || l2) {
            if (l1 && l2 ) {
                if (l1->val <= l2->val) {
                    cur->next = l1;
                    l1 = l1->next;
                } else {
                    cur->next = l2;
                    l2 = l2->next;
                }
                cur = cur->next;
            } else {
                cur->next = (l1) ? l1 : l2;
                break;
            }
        }
        return dummyhead->next;
    }

    ListNode* middlenode(ListNode* head) {
        ListNode* slow = head, * fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode* mid = middlenode(head);
        ListNode* l1 = head, * l2 = mid->next;
        mid->next = nullptr;
        return merge(sortList(l1),sortList(l2));
    }
};
```
```cpp
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2,ListNode*& tail) {
        ListNode* dummyhead = new ListNode(0);
        ListNode* cur = dummyhead;
        while (l1 || l2) {
            if (l1 && l2 ) {
                if (l1->val <= l2->val) {
                    cur->next = l1;
                    tail = l1;
                    l1 = l1->next;
                } else {
                    cur->next = l2;
                    tail = l2;
                    l2 = l2->next;
                }
                cur = cur->next;
            } else {
                if (l1) {
                    cur->next = l1;
                    while (l1) {
                        if (!l1->next) tail = l1;
                        l1 = l1->next;
                    }
                } else {
                    cur->next = l2;
                    while (l2) {
                        if (!l2->next) tail = l2;
                        l2 = l2->next;
                    }
                }
                break;
            }
        }
        return dummyhead->next;
    }

    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) return head;
        int len = 0;
        ListNode* tmp = head;
        while (tmp) {
            len++;
            tmp = tmp->next;
        }
        ListNode* dummyhead = new ListNode(0,head);
        for (int sublen = 1; sublen < len; sublen *= 2) {
            ListNode* prev = dummyhead,* curr = dummyhead->next;
            while (curr) {
                ListNode* l1 = curr, * l1_tail = curr;
                for (int i = 1; i < sublen && l1_tail->next; i++) {
                    l1_tail = l1_tail->next;
                }
                ListNode* l2 = l1_tail->next, * l2_tail = l1_tail->next;
                l1_tail->next = nullptr;
                prev->next = nullptr;
                for (int i = 1; i < sublen && l2_tail; i++) {
                    l2_tail = l2_tail->next;
                }
                ListNode* next = (l2_tail) ? l2_tail->next : l2_tail;
                if (next) {
                    l2_tail->next = nullptr;
                }
                ListNode* tail = nullptr;
                ListNode* tmp_merge = merge(l1,l2,tail);
                prev->next = tmp_merge;
                prev = tail;
                curr = next;
            }
        }
        return dummyhead->next;
    }
};
```