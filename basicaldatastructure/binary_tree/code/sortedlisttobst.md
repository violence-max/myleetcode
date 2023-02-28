题目描述：  
![image](/basicaldatastructure/binary_tree/image/image42.png)  
解题过程：  
很简单，5分钟ac  
从题解里学了两种方法，一种是快慢指针寻找链表的中间节点（精髓是快指针不能为最后一个节点或者快指针的下一个节点不能为最后一个节点时退出循环），一种中序遍历构建二叉搜索树（细节见代码部分）  
代码：（分治→分治+中序遍历）  
```cpp
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return nullptr;
        ListNode* slow = head, * fast = head,* pre = nullptr;
        while (fast->next != nullptr && fast->next->next != nullptr) {
            pre = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        TreeNode* root = new TreeNode(slow->val);
        if (pre) pre->next = nullptr;
        if (pre) root->left = sortedListToBST(head);
        if (slow->next) root->right = sortedListToBST(slow->next);
        return root;
    }
};
```
```cpp
class Solution {
public:
    int getlength(ListNode* head) {
        int ret = 0;
        for (; head != nullptr; ++ret,head = head->next)
            ;
        return ret;
    }
		
		//对head采用的是引用，不是拷贝
    TreeNode* buildtree(ListNode*& head ,int left, int right) {
        if (left > right) {
            return nullptr;
        }
        int mid = (left + right + 1) >> 1;
        TreeNode* root = new TreeNode();
        root->left = buildtree(head,left,mid - 1);
        root->val = head->val;
        head = head->next;
        root->right = buildtree(head,mid + 1, right);
        return root;
    }

    TreeNode* sortedListToBST(ListNode* head) {
        int length = getlength(head);
        return buildtree(head,0,length - 1);
    }
};
```