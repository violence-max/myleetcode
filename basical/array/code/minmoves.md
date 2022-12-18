题目描述：  
![image](/basical/array/image/image24.png)  
解题过程：用传统的各种有关弹性或者dp的方法对这道题根本无从下手，让我很难受地看了题解，然后发现，我去，这就是强无敌。  
第一步：构建零数组 
第二步：计算第一个花销  
第三步：滑动窗口 
我去，把所有1之间的0单独拉出来以及间两个1之间的0整体看待的思路简直骚得不行，受不了  
细节还是太多了，尤其是计算前缀和以及利用前缀和计算两个下标之间的和，还有把滑动窗口长度为奇数和偶数两种情况统一起来的方法实在是太难得了。 
代码： 
```cpp
class Solution {
public:
    vector<int> GenerateZeros(const vector<int> &nums) {
        int n = nums.size();
        int i = 0;
        vector<int> zeros;
        while (i < n && nums[i] == 0) i++;
        while (i < n) {
            int j = i + 1;
            while (j < n && nums[j] == 0) j++;
            if (j < n) {
                zeros.push_back(j - i - 1);
            }
            i = j;
        }
        return zeros;
    }

    vector<int> GeneratePre(const vector<int> &zeros) {
        vector<int> pre (1,0);
        for (int n : zeros) {
            pre.push_back(pre.back() + n);
        }
        return pre;
    }

    int GenerateSum(int left,int right,const vector<int> &pre) {
        return pre[right+1] - pre[left];
    }

    int minMoves(vector<int>& nums, int k) {
        vector<int> zeros = GenerateZeros(nums);
        int cost = 0;
        int right = k- 2;
        for (int i = 0; i < k - 1; i++) {
            cost += zeros[i] * min(i+1,right-i+1);
        }
        vector<int> pre = GeneratePre(zeros);
        int minCost = cost;
        int i = 1,j = k - 1;
        for (; j < zeros.size(); i++,j++) {
            int mid = (i + j) >> 1;
            cost -= GenerateSum(i-1,mid-1,pre);
            cost += GenerateSum(mid+k%2,j,pre);
            minCost = min(minCost,cost);
        }
        return minCost;
    }
};
```