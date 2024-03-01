题目描述：

![image](/basical/array/image/image105.png)

解决过程：
第361场周赛t3，没做出来

- 模+前缀和+哈希表

代码：

```
long long sum[100020];
class Solution {
public:
    long long countInterestingSubarrays(vector<int>& nums, int modulo, int k) {
        int n = nums.size();
        int now = 0;
        long long ans =  0;
        memset(sum, 0, sizeof sum);
        sum[0] = 1;
        for (int i = 0; i < n; ++i) {
            if (nums[i] % modulo == k) {
                now = (now + 1) % modulo;
            }
            if ((now - k + modulo) % modulo <= n) {
                ans += sum[(now - k + modulo) % modulo];
            }
            sum[now]++;
            // cout << i << ' ' << now << ' ' << ans << endl;
        }
        return ans;
    }
};
```