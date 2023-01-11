题目描述：  
![image](/basicaldatastructure/stackandquere/image/image15.png) 
解决过程：  
想了一个O(n^2)时间复杂度的代码，每次从后往前遍历寻找最大值，超时了，嘻嘻  
代码：  
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        int areaMax = 0;
        for (int i = 0; i < n; i++) {
            int minHeight = heights[i],width = 0;
            for (int j = i; j >= 0; j--) {
                minHeight = (heights[j] < minHeight) ? heights[j] : minHeight;
                width++;
                areaMax = max(areaMax,width*minHeight);
            }
        }
        return areaMax;
    }
};
```  
单调栈：这就是单调栈！找到一个值左右两边第一个比它小的值，然后除去这两个值的下标（减去2）之间的数据都是比这个值要大的，就可以直接用宽度乘以高！  
- code review  
单调栈的经典做法，居然就这么被我一笔带过，真的是暴殄天物啊。  
单调栈：找出每个数位上的值的左右两边的第一个比其大或者比起小的位置，那么在着两个位置之间的值就都比其小或者比其大  
以小单调栈为例：  
1. 设置左数组left，大小为n，初始值为-1，右数组，大小为n，初始值为n
2. 用一个栈stk进行辅助
3. 用for循环遍历待压入单调栈中的数值：若栈非空而且栈顶元素大于当前值，说明栈顶元素向右遇到了第一个比其小的元素，此时将右数组进行赋值并且将其弹出栈中，这个过程在while循环里面进行直到栈空或者栈顶元素小于或者等于当前值为止。while循环之后，若栈为空，说明栈中的每个值都比当前值大或者当前值左边不存在数，因此left数组对应位置置为-1，否则置为栈顶元素，则left数组对应位置为左边第一个小于或者等于的数字
4. 每次for循环都将当前数字入栈
要注意的是，我们无法通过一次遍历求出正确的左边界，因为我们在对左数组进行赋值时使用的判断条件其实是小于等于号，因为只有栈顶元素大于当前元素时才会对栈顶元素的右边界数组进行赋值，退出循环意味着有可能栈顶元素与当前元素是相等的。然而，这并不影响我们求解出正确的答案——如果遇到连续的高度相等的矩形，第一个矩形的左边界一定是对的，而右边界的判断是绝对正确的，因此第一个矩形求解出来的矩形的面积仍然是最大的。想要求解出正确的左数组和右数组，**必须进行从左到右和从右到左的两次遍历。**求解最大矩形的面积的部分就比较简单了，但绝对不像我第一次描述的那么简单，我觉得那样子描述对于这一题来说是侮辱的，因为根本没有讲到精髓。  
假设我们对于heights数组中的每一个值都求出了正确的left数组和right数组，那么，对于一个高度height而言，如果我们想要用这个高度当作是矩形的高度，这个矩形的最大宽度往左右两边能扩展到多少呢？假设l为左边第一个高度小于height的矩形，r为右边第一个高度小于height的矩形，那么，**在[l+1,r-1]范围内的矩形的高度就都大于等于height**了，因此这个最大的可以扩展到的宽度为(r-1) - (l+1) + 1 = r - l - 1，用这个宽度乘以height就得到了以height为高度能够得到的面积最大的矩形，依次以此规则对heights数组进行循环就可以得到最大面积了  
```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n,-1),right(n,n);
        stack<int> s;
        for (int i = 0; i < n; i++) {
            while (!s.empty() && heights[s.top()] > heights[i]) {
                right[s.top()] = i;
                s.pop();
            }
            left[i] = (s.empty()) ? -1 : s.top();
            s.push(i); 
        }
        int areaMax = 0;
        for (int i = 0; i < n; i++) {
            areaMax = max(areaMax,(right[i]-left[i]-1)*heights[i]);
        }
        return areaMax;
    }
};
```