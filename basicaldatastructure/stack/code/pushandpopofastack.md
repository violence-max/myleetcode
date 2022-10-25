题目描述：  
![image2](/basicaldatastructure/stack/image/image2.png)  
**隐藏条件：因为pushed时poped的排列，因此栈中不存在重复元素（否则无法确定一个排列）**  
解题过程：  
这道题自己做不了什么尝试，而且在学习阶段，还是以收集优秀思路减小时间成本为主。我看到这个题的例子就清楚这道题想考察什么了，但是我的脑子里只有那种很简单的办法，比如从pop数组开始遍历，现在push数组中找到对应的值，然后检查push数组中对应值或者pop数组正在遍历的值之后的元素是否符合逻辑，而且我还没有把这个逻辑想得非常彻底，看了题解之后发现原来可以用一个**辅助栈模拟栈压入和弹出的过程**。我认为这个才是正解，如果想知道是否符合逻辑，只需要模拟一遍过程看能不能成功就可以了。但是我觉得好像可以在失败了之后提前返回False？题解都是遍历完之后才进行判断，其实从条件选择的角度来讲也算是体骄傲返回False了吧，因为如果不符合逻辑应该会很快退出循环，因为两个栈在某时刻就对不上了。  
方法一：（O(n)空间复杂度解决）  
```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        stack,i = list(),0
        for elem in pushed:
            stack.append(elem)
            while stack and stack[-1] == popped[i]:
                stack.pop()
                i += 1
        return bool(len(stack) == 0)
```  
方法二：（O(1)空间复杂度解决）  
```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        i,j = 0,0
        for elem in pushed:
            pushed[i] = elem
            while i >= 0 and pushed[i] == popped[j]:
                j += 1
                i -= 1
            i += 1
        return bool(i == 0)
```  
方法二是天才的，不用辅助栈，直接用原来的push数组，同样是从一开始遍历，遇到pop数组的元素则不断更改j和i的值模拟弹出，弹出后i加一进行指针移动，下一次循环时将该位置的元素的值在迭代器里补上，最终i在0值处完成最后一个值的比较，然后加以退出循环，只需判断i是否等于0就可以得到结论  