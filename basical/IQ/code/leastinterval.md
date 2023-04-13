题目描述：  
![image](/basical/IQ/image/image29.png)  
解决过程：  
没有思路  
题解里有两种方法：  
- 模拟：每次选取最早可以执行任务中的剩余次数最多的任务执行，同时更新其下一次最早的可执行时间。
- 构造：使用n+1列的矩阵按照列排序的规则放置出现次数最多的任务，剩余的任务从倒数第二列开始往上排  
代码：（模拟→构造）  
```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char, int> freq;
        for (char ch: tasks) {
            ++freq[ch];
        }
        
        // 任务总数
        int m = freq.size();
        vector<int> nextValid, rest;
        for (auto [_, v]: freq) {
            nextValid.push_back(1);
            rest.push_back(v);
        }

        int time = 0;
        for (int i = 0; i < tasks.size(); ++i) {
            ++time;
            int minNextValid = INT_MAX;
            for (int j = 0; j < m; ++j) {
                if (rest[j]) {
                    minNextValid = min(minNextValid, nextValid[j]);
                }
            }
            time = max(time, minNextValid);
            int best = -1;
            for (int j = 0; j < m; ++j) {
                if (rest[j] && nextValid[j] <= time) {
                    if (best == -1 || rest[j] > rest[best]) {
                        best = j;
                    }
                }
            }
            nextValid[best] = time + n + 1;
            --rest[best];
        }

        return time;
    }
};
```  
```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char,int> freq;
        for (auto task : tasks) {
            freq[task]++;
        }
        int maxExcc = max_element(freq.begin(),freq.end(),[](const auto& u, const auto& v) {
            return u.second < v.second;
        })->second;
        int maxCount = accumulate(freq.begin(),freq.end(),0,[&](int a, const auto& u) {
            return a + (u.second == maxExcc);
        });
        return max((maxExcc - 1) * (n + 1) + maxCount, static_cast<int>(tasks.size()));
    }
};
```