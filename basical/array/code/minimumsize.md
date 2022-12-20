题目描述：  
![image](/basical/array/image/image26.png)  
解题过程：  
一开始想出来一招：均分然后插入数组（看了题解之后发现用优先队列可以更快地插入），但是尝试了过后发现均分并不是最优解。  
二分查找！  
找到一个y值看y值是否可以在有限的操作数内作为当个袋子里的最大值，最精华的应该是计算所有袋子里的球的操作次数的那一部分：`(x - 1) / y` ，这被看作是x在(0,y]、[y+1,2*y]……所进行的要划分为y的最少操作次数，强无敌！  
代码：  
```cpp
class Solution {
public:
    int minimumSize(vector<int>& nums, int maxOperations) {
        int left = 1,right = *max_element(nums.begin(),nums.end());
        int ans = 0;
        while (left <= right) {
            int mid = (left + right) >> 1;
            long long ops = 0;
            for (int x : nums) {
                ops += (x - 1) / mid;
            }
            if (ops <= maxOperations) {
                ans = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
};
```