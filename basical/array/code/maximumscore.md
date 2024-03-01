题目描述：

![image](/basical/array/image/image100.png)

解决过程：
第358场周赛，t4

学到了如何判断一个数有多少个质因数

代码：

```cpp
int cnt[100020];
const int MX = 100001;
int init = []() {
    for (int i = 2; i < MX; ++i) {
        if (cnt[i] == 0) {
            for (int j = i; j < MX; j += i) {
                cnt[j]++;
            }
        }
    }
    return 0;
}();

class Solution {
    const int mod = 1e9 + 7;
    long long pow(long long n, int x) {
        long long ret = 1;
        while (x) {
            if ((x & 1) == 1) ret = ret * n % mod;
            n = n * n % mod;
            x = x >> 1;
        }
        return ret;
    }
public:
    int left[100020], right[100020], id[100020];
    int maximumScore(vector<int>& nums, int k) {
        int n = nums.size();
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            right[i] = n;
            while (!stk.empty() && cnt[nums[stk.top()]] < cnt[nums[i]]) {
                right[stk.top()] = i;
                stk.pop();
            }
            left[i] = stk.empty() ? -1 : stk.top();
            // cout << i << ' ' << left[i] << ' ' << right[i] << endl;
            stk.push(i);
        }
        iota(id, id + n, 0);
        sort(id, id + n, [&](int a, int b){
            return nums[a] > nums[b];
        });
        long long ret = 1;
        for (int i = 0; i < n; ++i) {
            int idx = id[i];
            int tot = (idx - left[idx]) * (right[idx] - idx);
            // cout << idx << ' ' << nums[idx] << ' ' << left[idx] << ' ' << right[idx] << ' ' << tot << endl;
            if (tot >= k) {
                ret = ret * pow(nums[idx], k) % mod;
                break;
            }
            ret = ret * pow(nums[idx], tot) % mod;
            k -= tot;
        }
        return ret;
    }
};
```