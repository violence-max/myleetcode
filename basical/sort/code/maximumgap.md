题目描述：  
![image](/basical/sort/image/image3.png)  
解题过程：  
不会，看的题解  
1. 基数排序：感觉题解和代码写得很清楚，不过自己在为什么对buf数组进行赋值的时候需要对nums数组从后往前遍历存在疑惑，经过研究发现 ，由于nums数组是已经在低位排好序了的，因此从后往前遍历可以保证从高位，即9开始的数据在已经遍历过的位数里大小的顺序（例子是3000，4000，下一轮是万分位的排序，从前往后遍历会让3000在4000后面）
2. 桶排序：稍微有点晦涩，主要思想是一个证明：最大间距不小于d = (max - min) / n - 1，基于此划分(max - min) / d + 1个桶，加一的目的是防止对d进行相除向下取整的时候有小数部分。对于nums中的每个数num，都可以找到一个合适的编号：(num - min) / d，对于每个相邻的桶，很容易看出后面的桶的每个数据都会大于前面的桶，因此可以进行一系列操作  
代码：（基数排序→桶排序）  
```cpp
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) return 0;
        int maxval = *max_element(nums.begin(),nums.end());
        int exp = 1;
        vector<int> buf (n);
        while (maxval >= exp) {
            vector<int> cnt (10);
            for (int i = 0; i < n; i++) {
                int digit = (nums[i] / exp) % 10;
                cnt[digit]++;
            }
            for (int i = 1; i < 10; i++) {
                cnt[i] += cnt[i-1];
            }
            for (int i = n - 1; i >= 0; i--) {
                int digit = (nums[i] / exp) % 10;
                buf[cnt[digit] - 1] = nums[i];
                cnt[digit]--;
            }
            copy(buf.begin(),buf.end(),nums.begin());
            exp *= 10;
        }   
        int ret = 0;
        for (int i = 1; i < n; i++) {
            ret = max(ret,nums[i] - nums[i-1]);
        }
        return ret;
    }
};
```
```cpp
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) {
            return 0;
        }
        int minVal = *min_element(nums.begin(), nums.end());
        int maxVal = *max_element(nums.begin(), nums.end());
        int d = max(1, (maxVal - minVal) / (n - 1));
        int bucketSize = (maxVal - minVal) / d + 1;

        vector<pair<int, int>> bucket(bucketSize, {-1, -1});
        for (int i = 0; i < n; i++) {
            int idx = (nums[i] - minVal) / d;
            if (bucket[idx].first == -1) {
                bucket[idx].first = bucket[idx].second = nums[i];
            } else {
                bucket[idx].first = min(bucket[idx].first, nums[i]);
                bucket[idx].second = max(bucket[idx].second, nums[i]);
            }
        }

        int ret = 0;
        int prev = -1;
        for (int i = 0; i < bucketSize; i++) {
            if (bucket[i].first == -1) continue;
            if (prev != -1) {
                ret = max(ret, bucket[i].first - bucket[prev].second);
            }
            prev = i;
        }
        return ret;
    }
};
```