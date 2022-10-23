题目描述：

![image1](/basical/image/image1.png)

做题过程：

没做出来，一开始想的是从最外开始打印，然后打印到里层，但是写的循环不太对，没有控制好何时退出循环也没有想出来什么时候退出循环。看了题解题解也是用了一个层次的方法，但是它那个控制比我好得多。我一开始是这样想的：如果是一个m*n的矩阵，先打印第一行第0列到第n-2列，然后打印第n-1列第0行到第m-2行，然后打印第m行第n-1列到第1列，最后打印第0列第m-1行到第1行，然后再将控制行和列的循环数减一，即m和n都减一，通过判断加入空闲列表的数的个数是否和原来矩阵元素个数相等来退出循环。但是我发现这样子做不太行，因为当打印执行久了之后，控制的进入打印循环的条件数就永假了，因为我的判断条件里有`col ≤ n-2` 和`row ≤ m-2` 这样，如果需要这两个条件成立那么 m和n的值至少保证在2才能让这个条件判断有可能为真，因为矩阵的索引都是非零值，所以应该需要对m或者n小于2的情况做具体判断，这样子就花了很多功夫而且逻辑不太好想。一开始瞄了一眼题解，看到了标志矩阵，以为一下子就来灵感了，然后就马上改了一个版本，大致是改成了一个暴力版本，就是m和n的值不减一，通过与标志矩阵的判断来决定要不要打印当前元素，然而进入了死循环，因为觉得这个方法花的时间也太多了，所以果断放弃。然后来到了最终题解。

采用的是题解里的层次的方法，觉得一遍扫描就过很酷，但是题解里的扫描的顺序跟我不太一样：

![image2](/basical/image/image2.png)

这样子的扫描特别有灵性，只要在扫描到右下角的时候加一个判断语句：`if top < bottom and left < right` 来决定是不是要继续扫描就可以了，整个循环的判断条件是左指针没有大于右指针，上指针没有大于下指针。

代码：

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix or not matrix[0]:
            return []
        count,m,n = 0,len(matrix),len(matrix[0])
        top,left,bottom,right = 0,0,m-1,n-1
        result = []
        while top <= bottom and left <= right:
            for col in range(left,right+1):
                result.append(matrix[top][col])
            for row in range(top+1,bottom+1):
                result.append(matrix[row][right])
            if left < right and top < bottom:
                for col in range(right-1,left,-1):
                    result.append(matrix[bottom][col])
                for row in range(bottom,top,-1):
                    result.append(matrix[row][left])
            left,right,top,bottom = left+1,right-1,top+1,bottom-1
        return result
```