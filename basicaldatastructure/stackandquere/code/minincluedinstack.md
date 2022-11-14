题目描述：  
![image1](/basicaldatastructure/stack/image/image1.png)  
这道题的解题思路真的太妙了，一开始看到的时候我觉得跟我做过的LRU那道题的整体结构真的类似，然后我觉得python的有关init函数的东西都忘了，所以有点胆怯。上了一下厕所，我突然发现这个题好像可以写。我的起初的思路是记录一个最小值，需要的时候返回即可，但是只有一个最小值很有问题，比如，当前栈的最小值是栈顶的元素，但是栈顶元素被弹出了，那如何更新最小值呢？之前我做过类似的问题，但是忘记在哪个题目了，总而言之就是同样的用了一个方法来记录当前数据结构里具有某个特征的值，但是都不约而同的因为某种原因造成没有办法正常更新这个值。以后如果有类似的问题要注意了，因为现在有解决方法了：**观察数据来设计一个有效的数据结构**。在这道题里面就设计了一个辅助栈，利用如果栈里当前最小值不是栈顶元素则对栈顶元素弹出后无需对最小值进行更新的特性来存储栈中每个位置对应的最小值。  
这道题还让我知道了一些python代码的有趣的地方：  
1. `math.inf` 代表正最大浮点数
2. list的pop和push方法
3. list元素用-1来索引最后一个值
4. 想要在一个类里的其它地方引用init函数里定义的变量，则需要在init函数里定义变量时添加self前缀
代码： 
```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = list()
        import math
        self.min_stack = [math.inf]

    def push(self, x: int) -> None:
        self.stack.append(x)
        self.min_stack.append(min(x,self.min_stack[-1]))

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def min(self) -> int:
        return self.min_stack[-1]

# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.min()