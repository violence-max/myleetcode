题目描述：

![image](/algorithmn/dynamic_programming/image/image108.png)

解决过程：

第118场双周赛t4，没做出来
代码：

```cpp
class Solution {
public:
    int findMaximumLength(vector<int>& nums) {
        int n = nums.size();
        vector<long long> s (n + 1); // 前缀和(注意数据范围)
        vector<int> f (n + 1); // f[i] 表示 [0,i]区间内找到的最大非递减数组的长度
        vector<long long> last (n + 1); // last[i] 表示 [0,i]区间找到的最大非递减数组的长度的前提下最后一个数的最小值
        vector<int> q (n + 1); // 模拟双端队列，q[i]存放s[q[i]]+last[q[i]]的值，并且保证这个值是递增的
        int front = 0; // 双端队列的头
        int rear = 0; // 双端队列的尾
// cout << "here";
        for (int i = 1; i <= n; ++i) { // 计算f[i]
            s[i] = nums[i-1] + s[i-1];
            // cout << i << ' ' << s[i] << endl;
            while (front < rear && s[q[front + 1]] + last[q[front + 1]] <= s[i]) {
                // 找到使得s[q[front]] + last[q[front]] <= s[i] 的最大值front，相当于移除队头
                // 如果s[i] - s[j] >= last[j]，则f[i]可以从f[j]转移而来
                // 随着i的增大，f[i]也会增大(最后一个数总是可以合并到前一个数中，因此f[i] >= f[i-1])
                // 因此，应当找到使得s[q[front]] + last[q[front]] <= s[i] 的最大值front，那么可以使得f[i]从f[q[front]]转移而来，并且f[i]最大
                ++front;
            }

            f[i] = f[q[front]] + 1;
            last[i] = s[i] - s[q[front]];
            // cout << i << ' ' << f[i] << ' ' << last[i] << ' ' << front << ' ' << q[front] << endl;
            while (rear >= front && s[q[rear]] + last[q[rear]] >= s[i] + last[i]) {
                // 假设j < k，且s[j] + last[j] >= s[k] + last[k]
                // 那么，如果f[i]可以从f[j]转移得来，即s[i] - s[j] >= last[j]，f[i]就可以从f[k]转移得来
                // 因此，需要移除无法完成之后转移的元素
                --rear;
            }

            q[++rear] = i;
            // cout << 
        }
        return f[n];
    }
};
```