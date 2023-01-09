题目描述：  
![image](/basical/IQ/image/image11.png)  
解题过程：  
直接模拟  
题解里面有数学的解法，很牛，但是没心情看  
代码：  
```cpp
class Solution {
public:
    bool check(vector<int> perm) {
        int m = perm.size();
        for (int i = 0; i < m; i++) {
            if (perm[i] != i) {
                return false;
            }
        }
        return true;
    }
    
    int reinitializePermutation(int n) {
        vector<int> perm (n);
        iota(perm.begin(),perm.end(),0);
        int ans = 0;
        while (true) {
            vector<int> arr (n);
            for (int i = 0; i < n; i++) {
                if (i & 1) {
                    arr[i] = perm[n / 2 + (i - 1) / 2];
                } else {
                    arr[i] = perm[i / 2];
                }
            }
            ans += 1;
            if (check(arr)) {
                break;
            } else {
                perm = arr;
            }
        }
        return ans;
    }
};
```