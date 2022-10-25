题目描述：  
![image1](/basicaldatastructure/binary_tree/image/image1.png)    
做题过程：  
这道题，一开始看上去的时候觉得思路应该不是特别难，毕竟应该只是一个递归加遍历。然后我就写了一个初始版本：我想从根节点开始比较，不行再左子树然后右子树，但是我定睛一看我写的比较函数，我真的吐了，我比较的是两棵树值是否相等。之后我就陷入了沉思，我觉得自己没有办法写出一个判断B数是不是A树子结构的函数，然后就很痛苦。之后在题解上得到了精确的解释：  
1. B树空，则证明B树已经完全比较结束，返回True
2. A树空，则证明A树已经溢出叶子结点，返回False
3. A树和B树的值不相等，自然返回False 
然后就是顺理成章地递归比较左子树和右子树就可以了  
先看代码：  
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def compareTree(self,A:TreeNode,B:TreeNode) -> bool:
        if not B:
            return True
        elif not A or A.val != B.val:
            return False
        return self.compareTree(A.left,B.left) and self.compareTree(A.right,B.right)

    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        return bool(A and B) and (self.compareTree(A,B) or self.isSubStructure(A.left,B) or self.isSubStructure(A.right,B))
```  
这个代码有很多写得比较有亮点的地方：  
1. 用布尔表达式代替直接的True或者False语句以节省if else语句来减少行数
2. 返回语句逻辑清晰
3. 比较函数行子顺序明确，先看B是否空再看A是否空，颠倒过来有较大差距