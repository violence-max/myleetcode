题目描述：

![image](/basicaldatastructure/setandmap/image/image12.png)

解题过程：

貌似一小时做出来了

1. 使用哈希map默认会添加元素
2. 这题需要考虑数组长度为0的情况
3. 可以直接根据数据量枚举2的幂
4. 可以直接枚举桶

代码：

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;

    int countPairs(vector<int>& deliciousness) {
        int n = deliciousness.size();
        if (n == 1) {
            return 0;
        }
        sort(deliciousness.begin(), deliciousness.end());
        unordered_map<int, int> cnt;
        for (auto i : deliciousness) {
            cnt[i]++;
        }
        long long ans = 0;
        for (auto& [t, f] : cnt) {
            for (int k = pow(2, 0); k <= pow(2, 21); k *= 2) {
                if (cnt.count(k - t) && cnt[k - t] > 0) {
                    if (k - t == t) {
                        int fre = f;
                        ans = (ans + (long long)(fre - 1) * fre / 2) % MOD;
                    } else {
                        ans = (ans + (long long)f * cnt[k - t]) % MOD;
                    }
                }
            }
            f = 0;
        }
        return ans;
    }
};
```