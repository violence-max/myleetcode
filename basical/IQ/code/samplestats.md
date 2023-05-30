题目描述：

![image](/basical/IQ/image/image40.png)

解决过程：

中位数太恶心了就不写了，题解很精妙

代码：

```bash
class Solution {
public:    
    vector<double> sampleStats(vector<int>& count) {
        int n = count.size();
        int total = accumulate(count.begin(), count.end(), 0);
        double mean = 0.0;
        double median = 0.0;
        int minnum = 256;
        int maxnum = 0;
        int mode = 0;

        int left = (total + 1) / 2;
        int right = (total + 2) / 2;
        int cnt = 0;
        int maxfreq = 0;
        long long sum = 0;
        for (int i = 0; i < n; i++) {
            sum += (long long)count[i] * i;
            if (count[i] > maxfreq) {
                maxfreq = count[i];
                mode = i;
            }
            if (count[i] > 0) {
                if (minnum == 256) {
                    minnum = i;
                }
                // 直接取最后一个次数大于0的数当作最大值
                maxnum = i;
            }
            // 1. 奇数的情况：
            // right 和 left相等，因此会进入两次条件语句，之后median / 2得到正确答案
            // 2. 偶数的情况：
            // right比left大1， 第一次进入left的条件语句，第二次进入right的条件语句或者
            // 连续进入right的条件语句和left的条件语句，同样也得到正确答案
            if (cnt < right && cnt + count[i] >= right) {
                median += i;
            }
            if (cnt < left && cnt + count[i] >= left) {
                median += i;
            }
            cnt += count[i];
        }
        mean = (double) sum / total;
        median = median / 2.0;
        return {(double)minnum, (double)maxnum, mean, median, (double)mode};
    }
};
```