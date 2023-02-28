题目描述：  
![image](/basical/IQ/image/image14.png)  
解题过程：  
一开始拿到这道题我直接回溯，第七个测试用例直接超时了。看了以下数组的长度，10^5我去，还回溯，求个子集估计都报废了。而且一开始我想练习一下辗转相除法，但是忘记算法流程了，还花了十分钟才撸出来  
看了一下题解，我感觉是真的牛逼，又是强无敌的逆向思维。既然求解子集的路行不通，不妨从答案方面开始想。一个数组的最大公约数的取值范围只可能是最小值元素到最大值元素之间。于是可以从最大公约数开始枚举。但是，怎么确定一个数是否是一个数组的某个子序列的最大公约数呢？如果p是子序列{a0,a1,a2,……an_1}的最大公约数，则ai可以整除p。因此在枚举p的时候，先设置初始的最大公约数为0，因为0和所有数的最大公约数均为该数本身。然后枚举p的倍数，如果这个倍数存在于数组中，就更新一次最大公约数的值。如果更新出来的值和p相等，则说明数组中存在一个序列并且其最大公约数为p。  
这种方法还存在着一种优化循环的方法。对于数组nums，其中的每个数单独拿出来作为子序列就可以得到一个最大公约数本身。那么我们可以直接考虑长度大于等于2的子序列的最大公约数x，且经过简单的推导可以得知x一定不在nums中，否则x可以直接作为长度为1的子序列，并且提供一个最大公约数即其本身。那么，如果x是nums的某个子序列的最大公约数，则这个子序列必须包含两个数2x和3x，因此，对于最大公约数的枚举只需要到数组nums对3x的高斯向下取整就好了，否则3x必定不在nums中。这其中给的算法逻辑的实现相较于法一还是有细微的调整的，因为要求x不能在nums中。实际实现的时候一开始的答案不能直接初始化为n，因为nums中有重复的元素。。。  
代码：（常规循环→优化循环）  
```cpp
class Solution {
public:
    int countDifferentSubsequenceGCDs(vector<int>& nums) {
        int n = nums.size(),maxVal = *max_element(nums.begin(),nums.end());
        vector<bool> occured (maxVal+1);
        for (int n : nums) occured[n] = true;
        int ans = 0;
        for (int i = 1; i <= maxVal; i++) {
            int subGcd = 0;
            for (int j = i; j <= maxVal; j += i) {
                if (occured[j]) {
                    subGcd = gcd(subGcd,j);
                }
                if (subGcd == i) {
                    ans += 1;
                    break;
                }
            }
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    int countDifferentSubsequenceGCDs(vector<int>& nums) {
        int n = nums.size(),maxVal = *max_element(nums.begin(),nums.end());
        vector<bool> occured (maxVal+1);
        int ans = 0;
        for (int n : nums) {
            if (!occured[n]) {
                occured[n] = true;
                ans += 1;
            }
        }
        for (int i = 1; i <= maxVal / 3; i++) {
            if (occured[i]) continue;
            int subGcd = 0;
            for (int j = 2 * i; j <= maxVal; j += i) {
                if (occured[j]) {
                    subGcd = gcd(subGcd,j);
                }
                if (subGcd == i) {
                    ans += 1;
                    break;
                }
            }
        }
        return ans;
    }
};
```