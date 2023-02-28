题目描述：  
![image](/basical/array/image/image47.png)  
解决过程：  
思考片刻就把答案给敲出来了，但是一开始敲的时候没有注意，我前半部分的偶数和还有奇数和都被改变了，发现错误后我立即用tmp变量进行修改。灵神是真的强，用一个变量就解决了四个变量，主要思路如下：  
1. 假设odd2和even1分别为前半部分的奇数和与偶数和，odd1和even1分别为后半部分的奇数和与偶数和
2. 那么令s2 = odd2 - even2，s1 = odd1 - even1，那么当s1 == s2时会获得一个方案数
3. 那么令s = s2 - s1，一开始s1为0，因此s等于整个数组的奇数和减去偶数和；开始从i = 0开始寻找方案数的个数时，如果i为奇数，那么前半部分的奇数部分减去nums[i]，如果i为偶数，那么前半部分的偶数和要加上该数（因为在s2的表示里面偶数和是负的，加上相当于减去）
4. 比较s == 0
5. 处理s1：由于s1前面有个负号，因此遇到奇数是减，遇到偶数是加
代码：（我的→灵神）  
```cpp
class Solution {
public:
    int waysToMakeFair(vector<int>& nums) {
        int oddsum = 0, evensum = 0, n =nums.size();
        for (int i = 0; i < n; i++) {
            if (i & 1) {
                oddsum += nums[i];
            } else {
                evensum += nums[i];
            }
        }
        int ans = 0, oddsum_behind = 0, evensum_behind = 0;
        for (int i = n - 1; i >= 0; i--) {
            int oddsum_tmp = oddsum, evensum_tmp = evensum;
            if (i & 1) {
                oddsum = oddsum - nums[i];
                oddsum_tmp = oddsum + evensum_behind;
                evensum_tmp += oddsum_behind;
                oddsum_behind += nums[i];
            } else {
                evensum = evensum - nums[i];
                evensum_tmp = evensum + oddsum_behind;
                oddsum_tmp += evensum_behind;
                evensum_behind += nums[i];
            }
            if (oddsum_tmp == evensum_tmp) ans++;
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    int waysToMakeFair(vector<int>& nums) {
        int n = nums.size(), s = 0, ans = 0;
        for (int i = 0; i < n; i++) {
            s += (i & 1) ? nums[i] : -nums[i];
        }
        for (int i = 0; i < n; i++) {
            s -= (i & 1) ? nums[i] : -nums[i];
            ans += s == 0;
            s -= (i & 1) ? nums[i] : -nums[i];
        }
        return ans;
    }
};
```