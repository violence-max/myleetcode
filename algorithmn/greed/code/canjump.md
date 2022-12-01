题目描述：  
![image](/algorithmn/greed/code/canjump.md)  
解题过程：  
完全只有枚举啊递归啊回溯啊这种笨办法，一看题解，真的是巨贪心无比，维护一个最远能够到达的距离，强无敌啊。看来我还不够贪心。  
代码：  
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int length = nums.size();
        int count = 0;
        for (int i = 0; i < length; i++) {
            if (i <= count) {
                count = max(count,i+nums[i]);
                if (count >= length-1) return true;
            }
        }        
        return false;
    }
};
```