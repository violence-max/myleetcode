题目描述：  
![image](/basicaldatastructure/stackandquere/image/image8.png)  
解题过程：  
一开始只想到了暴力法，我觉得我的暴力法还是比较优化的，因为我用了一个`pos` 变量来记录最大值的位置，所以只有在最大值刚好被移出时才需要重新寻找最大值，但是还是超时了。而且我在做最大值的时候自己定义了一个寻找范围为k的数组的最大值，犯了两个错误，第一个是初始化`result` 的时候没有赋值为`nums[start]` 而是赋值为`nums[0]` ;第二个错误是没有考虑到如果k值为1，pos值不会被赋值（一开始我写的判断最大值条件是大于号不是大于等于，一般大于等于就够了，但是这里不行，因为这里要对pos赋初值）  
看了题解，发现了三种方法：  
一、优先队列方法：使用由堆为基础构建的优先队列，每次插入和弹出的时候都会自动排序，最大的元素始终在队头，而且还能记录元素的位置，很有感觉。  
自己实现的时侯出了几个小问题，因为插入的是一个int对int型的标签，所以使用的是emplace方法，但我一开始使用的是push方法  
我本来以为可以按照暴力法的思路，只有当最大值和当前窗口最靠前的数值的下标之差为k时才需要pop，实则不然，因为对于每个数我都会加入队列中，所以对于所有处于topo位置且下标之差大于等于k的值都需要pop。而且有一个测试用例还针对了队列是否为空的情况，所以要加上当队列不为空时这个条件进行循环pop  
二、单调队列方法  
这个方法是寂寞的，看了这个方法以后又学了一个api，deque，迄今为止唯一一个已知的具有从前面和后面都又pop方法和push方法的api，很爽。  
大题思想就是在队列非空的情况下，如果队列尾部的值小于当前值，就把队列尾部的值弹出，然后在队列尾部加入当前值。这样，就得到了单调递减的一个队列，同理用单调递增也可以做。然后队列最前面的值就是最大值，加入答案之前应该做的判断是队列头部的值的下标是否在滑动窗口的范围内，所以同样需要一个while循环来把在队列头部的最大值且下标超过滑动窗口范围的值给筛选掉。  
三、分块+预处理  
这个方法有空再看吧。  
代码：  
一、暴力  
```cpp
class Solution {
public:
    int findTheMaxNum(vector<int> nums,int start,int end,int &pos) {
        int result = nums[start];
        for (int i = start; i <= end; i++) {
            if (nums[i] >= result) {
                result = nums[i];
                pos = i;
            }
        }
        return result;
    }
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int pos;
        int maxNum = findTheMaxNum(nums,0,k-1,pos);
        vector<int> result;
        result.push_back(maxNum); 
        for (int j = k; j < (int)nums.size(); j++) {
            if ((j - pos) == k) {
                maxNum = findTheMaxNum(nums,j-k+1,j,pos);
            } else if (nums[j] > maxNum) {
                maxNum = nums[j];
                pos = j;
            }
            result.push_back(maxNum);
        }
        return result;
    }
};
```  
二、优先队列  
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        priority_queue<pair<int,int>> q;
        for (int i = 0; i < k; i++) {
            q.emplace(nums[i],i);
        }
        vector<int> result = {q.top().first};
        for (int j = k; j < nums.size(); j++) {
            while (!q.empty() && j - q.top().second >= k) {
                q.pop();
            }
            q.emplace(nums[j],j);
            result.push_back(q.top().first);
        }
        return result;
    }
};
```  
三、单调队列  
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> q;
        for (int i = 0; i < k; i++) {
            while (!q.empty() && nums[i] >= nums[q.back()]) {
                q.pop_back();
            }
            q.push_back(i);
        }

        vector<int> res = {nums[q.front()]};
        for (int j = k; j < nums.size(); j++) {
            while (!q.empty() && nums[j] >= nums[q.back()]) {
                q.pop_back();
            }
            q.push_back(j);
            while (q.front() <= j - k) {
                q.pop_front();
            }
            res.push_back(nums[q.front()]);
        }
        return res;
    }
};
```