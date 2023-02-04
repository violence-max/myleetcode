题目描述：  
![image](/basical/array/image/image49.png)  
解决过程：  
一开始用了前缀数组乘积，但是不符合进阶版的O(1)的空间复杂度的要求。实际的优化非常秀，代码写得非常清楚了，可以直接看代码  
代码：（前缀数组→优化）  
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> preproduct(n), reverse_preproduct(n);
        int left = 0, right = n - 1;
        while (left < n && right >= 0) {
            preproduct[left] = left == 0 ? nums[left] : preproduct[left-1] * nums[left];
            reverse_preproduct[right] = right == n - 1 ? nums[right] : reverse_preproduct[right+1] * nums[right];
            // cout << preproduct[left] << ' ' << reverse_preproduct[right] << endl;
            left++; right--;
        }
        vector<int> ans (n);
        for (int i = 0; i < n; i++) {
            int pre = i > 0 ? preproduct[i-1] : 1;
            int reverse_pre = i < n - 1 ? reverse_preproduct[i+1] : 1;
            ans[i] = pre * reverse_pre;
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int length = nums.size();
        vector<int> answer(length);

        // answer[i] 表示索引 i 左侧所有元素的乘积
        // 因为索引为 '0' 的元素左侧没有元素， 所以 answer[0] = 1
        answer[0] = 1;
        for (int i = 1; i < length; i++) {
            answer[i] = nums[i - 1] * answer[i - 1];
        }

        // R 为右侧所有元素的乘积
        // 刚开始右边没有元素，所以 R = 1
        int R = 1;
        for (int i = length - 1; i >= 0; i--) {
            // 对于索引 i，左边的乘积为 answer[i]，右边的乘积为 R
            answer[i] = answer[i] * R;
            // R 需要包含右边所有的乘积，所以计算下一个结果时需要将当前值乘到 R 上
            R *= nums[i];
        }
        return answer;
    }
};
```