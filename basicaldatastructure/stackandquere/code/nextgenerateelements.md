题目描述：  
![image](/basicaldatastructure/stackandquere/image/image13.png)  
解题过程：  
就是一个遍历2*n的求余循环，注意数组越界问题，所以每次拿索引都要进行求余  
代码：  
```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans (n,-1);
        stack<int> s;
        for (int i = 0; i < 2 * n; i++) {
            int num = nums[i % n];
            while (!s.empty() && num > nums[s.top() % n]) {
                ans[s.top() % n] = num;
                s.pop();
            }
            s.push(i);
        }
        return ans;
    }
};
```