题目描述：  
![image](/basicaldatastructure/stackandquere/image/image14.png)  
这道题是真的好玩，直接看题解，因为脑子过载了想不出来  
动态规划：两遍扫描得到左右最大值，然后一遍扫描拿到每个位置降雨量，贼寂寞  
单调栈：保证栈是递减的，不严格，栈中至少存在两个元素的时候才考虑中间哪个元素的位置可以接的雨水，用遍历到的值和栈顶的值作比较，若大于，则弹出栈顶，此时栈为空则退出循环继续遍历下一个值，否则再拿到栈顶元素，然后用弹出的值的两边的最小最大高度减去弹出的高度  
双指针：最无敌，直接从左向右从右向左分别移动最高矩形，差值即为填充的雨水  
- code review  
**动态规划：**  
1. 定义left数组和right数组，left数组记录height数组每个位置上左边最大的矩形的高度，right数组记录height数组每个位置上右边最大的矩形的高度。要得到这两个数组，必须进行从左往右和从右往左的两次遍历
2. 一次遍历height数组求解能够接到的雨水的数量：对于height数组数位i而言，其能够接到的雨水的数量可以表示为左边矩形的最大高度和右边矩形的最大高度的最小值减去数位i上的矩形高度（这应该很好理解吧）  
**单调栈：**  
精髓是保证栈底元素到栈顶元素是单调递减的  
1. 定义一个栈stk
2. 用for循环遍历height数组，对于数位i上的值而言，如果当前栈非空而且height[i]大于栈顶元素，则进入while循环，因此此时栈顶元素位置可能可以接到雨水。用top变量接住栈顶元素的值，再弹出栈顶元素，若此时栈为空，则说明栈顶元素左侧不存在比起高度更高的矩形，那么栈顶元素的位置不能接到雨水，因此直接跳出while循环
3. 在while循环外直接将当前数位上的值入栈
4. 衔接第2步，若弹出栈顶元素后栈不为空，结合栈中的元素从栈底到栈顶是单调减的，则**此时**的栈顶元素和当前数位i上的值都大于top位置上的值，说明有一段可以接住雨水的区域：我们用left接住**此时**栈顶元素的值，则可以接住雨水的区域应该为**left与i之间**的区域，而高度应该是left和i位置上的最小高度减去top位置上的高度，直接将计算出来的能够接住雨水的数量加到答案中即可  
for循环遍历结束之后即可得到答案  
单调栈的方法是比较抽象的。因为栈中的元素从栈底到栈顶是单调减的，当遇到一个比栈顶元素要大的元素时，如果栈顶元素左侧存在比起高度更高的矩形，即栈顶元素弹出后栈非空，则**此时**的栈顶元素和当前遍历到的矩形夹住的区域即为可以接到雨水的区域，而之所以强调此时，是因为需要进行一次元素的弹出并且要求弹出以后栈仍然具有栈顶元素（非空）  
**双指针：**  
这个方法是最抽象且最容易理解的方法，可以直接用形式化的语言进行描述。  
一开始左指针left指向0，右指针right指向height数组的末尾元素位置n-1，左边的最大高度leftmax为0，右边的最大高度为rightmax为0。  
方法的精髓是不断移动左边高度最高的矩形和右边高度最高的矩形，若高度最高的矩形和当前位置上的矩形的高度存在差值，则此差值即为当前位置可以接到的雨水的数量。当left和right相遇时不可能接到雨水，直接退出while循环就好了。  
而判断指针移动的条件则可以直接通过比较left位置上矩形的高度和right位置上矩形高度的大小，比如，若height[left] < height[right]，则将左边矩形最大高度减去当前矩形的高度差加到答案中，若当前矩形即为左边矩形的最大高度，则加上0，不影响答案正确性，若左边最大高度的矩形在left的左边，则该最大高度的矩形和右边的矩形之间可以将left位置上的矩形夹住，左边最大高度的矩形的高度减去left位置上的矩形的高度则可以直接加到答案中，然后往右移动左指针。若left位置上的矩形高度和right位置上的矩形高度的大小比较是另一种情况，则选择判断右边的矩形。  
这个方法总感觉很tricky，但是难以描述其tricky的地方。让我觉得最有趣的是左右指针的移动总时保持着 一种“**默契**”，因为当左指针或者右指针遇到一个比较高的矩形时，另外一边得矩形得高度往往都会小于该举行高度，导致另外一边不断地朝着中间移动，直到遇到一个比该矩形更高的矩形为止。想象一下，就感觉是两个很高的矩形不断夹住中间一堆高度比较低的矩形来形成雨水。  
我好像找到一种更好的描述方法了：每次指针发生移动，不管是左边的指针发生移动还是右边，一定满足的条件是当前位置上的矩形的高度小于另外一边位置上的矩形的高度，而当前位置一侧的最高的高度要么是当前位置本身，要么是之前的某个位置。而当前位置一侧是从哪里转化来的呢？只能是之前的位置。说明如果左侧和右侧的矩形的高度大小关系满足某一侧小于另一侧时，较小的那一侧的最大高度也较小，那么就可以用这个最大高度不断减去当前遍历到的位置上的矩形的高度来形成雨水，直到遇到一个比另一侧的最大高度更高的矩形为止。  
代码：（动态规划→单调栈→双指针）  
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if (n == 0) {
            return 0;
        }
        vector<int> leftMax(n);
        leftMax[0] = height[0];
        for (int i = 1; i < n; ++i) {
            leftMax[i] = max(leftMax[i - 1], height[i]);
        }

        vector<int> rightMax(n);
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            rightMax[i] = max(rightMax[i + 1], height[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += min(leftMax[i], rightMax[i]) - height[i];
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        stack<int> stk;
        int n = height.size();
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && height[i] > height[stk.top()]) {
                int top = stk.top();
                stk.pop();
                if (stk.empty()) {
                    break;
                }
                int left = stk.top();
                int currWidth = i - left - 1;
                int currHeight = min(height[left], height[i]) - height[top];
                ans += currWidth * currHeight;
            }
            stk.push(i);
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        int left = 0,right = n - 1,leftMax = 0,rightMax = 0;
        int ans = 0;
        while (left < right) {
            leftMax = max(leftMax,height[left]);
            rightMax = max(rightMax,height[right]);
            if (height[left] < height[right]) {
                ans += leftMax - height[left++];
            } else {
                ans += rightMax - height[right--];
            }
        }
        return ans;
    }
};
```