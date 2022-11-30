题目描述：  
![image](/algorithmn/greed/image2.png)  
解题过程：  
我真的无语了，以后做题不能做那么久了，真的是效率巨低无比，做不了几个题就没时间了，相信自己的学习能力吧。虽然debug很快乐（难受死）。用回溯枚举这道题的，哈哈，刚做完回溯真的是什么都枚举，然后疯狂写函数，最后debug发现回溯的时候又是没有成功进入到下一个阶段而是，类似于循环没写明白吧。  
最后算法肯定没问题，枚举，但是超时了。  
看了题解，有动态规划、优化的动态规划和贪心，看了贪心，只能说强无敌，有机会再看动态规划吧，累了。  
代码：  
暴力：  
```cpp
class Solution {
private:
    vector<bool> flag;
    bool findFlag = false;
public:
    bool check(vector<int> nums) {
        if (nums.size() == 1) return true;
        int start = nums[1] - nums[0];
        if (start == 0) return false;
        bool flag = (start > 0) ? true : false;
        bool cflag;
        int cint;
        for (int i = 1; i < nums.size() - 1; i++) {
            cint = nums[i+1] - nums[i];
            if (cint == 0) return false;
            if ((cflag = (cint > 0) ? true : false) != !flag) return false;
            else flag = !flag;
        }
        return true;
    }

    void excuteNums(vector<int> nums) {
        vector<int> ans;
        for (int i = 0; i < flag.size(); i++) {
            if (flag[i]) ans.push_back(move(nums[i])); 
        }
        if (check(ans)) {
            findFlag = true;
        }
    }

    void excuteFlag(int miss,vector<int> nums,int left,int right) {
        if (miss == 0) {
            excuteNums(nums);
            if (findFlag) return;
        } else if (miss == 1) {
            for (int i = left; i <= right; i++) {
                flag[i] = false;
                excuteNums(nums);
                if (findFlag) return;
                flag[i] = true;
            }
        } else if (miss == 2) {
            for (int i = left; i <= right - miss + 1; i++) {
                flag[i] = false;
                for (int j = i + 1; j <= right; j++) {
                    flag[j] = false;
                    excuteNums(nums);
                    if (findFlag) return;
                    flag[j] = true;
                }
                flag[i] = true;
            }
        } else {
            for (int i = left; i <= right - miss + 1; i++) {
                flag[i] = false;
                excuteFlag(miss-1,nums,i+1,right);
                if (findFlag) return;
                flag[i] = true;
            }
        }
    }

    int wiggleMaxLength(vector<int>& nums) {
        int length = nums.size();
        if (length < 2) return length;
        flag.resize(length,true);
        for (int miss = 0; miss <= length - 1; miss++) {
            excuteFlag(miss,nums,0,length-1);
            if (findFlag) return length - miss;
        }
        return -1;
    } 
};
```  
贪心：  
```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int length = nums.size();
        if (length < 2) return length;
        int ret = (nums[1] - nums[0] == 0) ? 1 : 2;
        int prediff = nums[1] - nums[0];
        for (int i = 2; i < length; i++) {
            int diff = nums[i] - nums[i-1];
            if ((diff > 0 && prediff <= 0) || (diff < 0 && prediff >= 0)) {
                ret++;
                prediff = diff;
            }
        } 
        return ret;
    }
};
```