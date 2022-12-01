题目描述：  
![image](/algorithmn/greed/image/image6.png)  
解题过程：  
今天都不在状态，哎，但是，要加油！！！！  
这道题典型的简单题但是自己心不在焉，要卯足劲才行。在思考逻辑的时候忽略了很多事情，包括记录最大负数的下标，比较最小正数和最大附属的时候忘记加负号，对最大负数进行两次翻转的时候对要减去两次最大负数的值的逻辑思考了很久。对循环一遍数组之后k还有剩余的情况没有考虑到。  
看了题解之后发现还有空间换时间的哈希策略，强无敌。写一哈。  
代码：  
排序：  
```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
        int sum = 0;
        int lastminus = -1;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] < 0 && k > 0) {
                sum += - nums[i];
                lastminus = i;
                k--;
            } else if (nums[i] == 0 && k > 0) {
                k = 0;
            } else {
                if (k % 2 == 1) {
                    if (lastminus != -1  && nums[i] > - nums[lastminus]) sum += (nums[i] + 2 * nums[lastminus]);
                    else sum += - nums[i];
                    k = 0;
                } else {
                    sum += nums[i];
                }
            }
        }
        if (k != 0){
            if (k % 2 == 0) return sum;
            else return sum + 2 * nums[lastminus];
        } 
        return sum;
    }
};
```  
空间换时间：  
```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        unordered_map<int,int> freq;
        for (int n : nums) {
            freq[n]++;
        }
        int ans = accumulate(nums.begin(),nums.end(),0);
        for (int i = -100; i < 0; i++) {
            if (freq[i]) {
                int ops = min(k,freq[i]);
                ans += (-i) * ops * 2;
                freq[i] -= ops;
                freq[-i] += ops;
                k -= ops;
                if (k == 0) break;
            }
        }
        if (k > 0 && k % 2 == 1 && !freq[0]) {
            for(int i = 1; i <= 100; i++) {
                if (freq[i]) {
                    ans -= 2 * i;
                    break;
                }
            }
        }
        return ans;
    }
};
```