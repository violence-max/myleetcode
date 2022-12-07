题目描述：  
![image](/algorithmn/dynamic_programming/image/image27.png)  
解决过程：只想到了双重循环的方法，而且还盲目地以为可以提前跳跃用的whiile循环，实际上不能，只能一个一个试。  
代码：  
滑动窗口，不是完全理解，但是感觉强无敌  
```cpp
class Solution {
public:
    int patern(vector<int> &nums1,vector<int> nums2,int addr1,int addr2,int len) {
        int ret = 0,k = 0;
        for (int i = 0; i < len; i++) {
            if (nums1[addr1+i] == nums2[addr2+i]) {
                k++;
            } else {
                k = 0;
            }
            ret = max(ret,k);
        }
        return ret;
    }

    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(),n = nums2.size();
        int ret = 0;
        for (int i = 0; i < m; i++) {
            int len = min(m-i,n);
            int maxlen = patern(nums1,nums2,i,0,len);
            ret = max(ret,maxlen);
        }
        for (int i = 0; i < n; i++) {
            int len = min(n-i,m);
            int maxlen = patern(nums1,nums2,0,i,len);
            ret = max(ret,maxlen);
        }
        return ret;
    }
};
```  
动态规划   最强公共前缀，强无敌  
```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(),n = nums2.size();
        vector<vector<int>> dp(m+1,vector<int> (n+1,0));
        int ret = 0;
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                dp[i][j] = nums1[i] == nums2[j] ? dp[i+1][j+1] + 1 : 0;
                ret = max(ret,dp[i][j]);
            }
        }
        return ret;
    }
};
```  
自己的超时破代码  
```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size(),m = nums2.size();
        int len = 0;
        int result = 0;
        int i = 0;
        while (i < m){
            int start2,j = 0;
            while (j < n) {
                int start1 = j;
                start2 = i;
                while ((start1 < n && start2 < m) && nums2[start2] == nums1[start1]) {
                    start2++; start1++;
                }
                len = start2 - i;
                result = max(result,len);
                if (start1 == n) break;
                j++;
            }
            if (start2 == m) return result; 
            i++;
        }
        return result;
    }
};
```