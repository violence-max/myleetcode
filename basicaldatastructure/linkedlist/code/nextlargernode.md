题目描述：  
![image](/basicaldatastructure/linkedlist/image/image22.png)  
解决过程：  
- 单调栈：
1. 先计算链表的长度，并且将每个链表与其索引值建立映射
2. 用单调栈维护一个非递增序列，遇到比栈顶大的元素不断弹出直到栈为空或者栈顶元素大于等于当前元素为止  
代码：（我的→题解(直接计算，没有预处理)）  
```cpp
class Solution {
public:
    vector<int> nextLargerNodes(ListNode* head) {
        unordered_map<ListNode*, int> map;
        ListNode* tmp = head;
        int len = 0;
        while (tmp) {
            map[tmp] = len++;
            tmp = tmp->next;
        }

        ListNode* cur = head;
        vector<int> arr (len);
        stack<ListNode*> stk;
        while (cur) {
            while (!stk.empty() && cur->val > stk.top()->val) {
                arr[map[stk.top()]] = cur->val;
                stk.pop();
            }
            stk.push(cur);
            cur = cur->next;
        }
        return arr;
    }
};
```  
```cpp
class Solution {
public:
    vector<int> nextLargerNodes(ListNode* head) {
        vector<int> ans;
        stack<pair<int, int>> s;

        ListNode* cur = head;
        int idx = -1;
        while (cur) {
            ++idx;
            ans.push_back(0);
            while (!s.empty() && s.top().first < cur->val) {
                ans[s.top().second] = cur->val;
                s.pop();
            }
            s.emplace(cur->val, idx);
            cur = cur->next;
        }

        return ans;
    }
};
```