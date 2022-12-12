题目描述：  
![image](/basical/string/image/image16.png)  
解题过程：  
直接暴力，然后超时  
代码：  
暴力解法  
```cpp
class Solution {
public:

    int beautysubstr(string s) {
        unordered_map<char,int> pMap;
        int MAX = INT_MIN,MIN = INT_MAX;
        for (const auto &c : s) {
            pMap[c]++;
        }
        for (auto &map : pMap) {
            MAX = (map.second > MAX) ? map.second : MAX;
            MIN = (map.second < MIN) ? map.second : MIN;
        }
        return MAX - MIN;
    }

    int beautySum(string s) {
        int n = s.size();
        int sum = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                sum += beautysubstr(s.substr(i,j-i+1));
            }
        }
        return sum;
    }
};
```  
官解：利用了之前子串计算出来的数字的频率对后续子串的美丽值进行计算  
```cpp
class Solution {
public:
    int beautySum(string s) {
        int n = s.size();
        int size = 26;
        int res = 0;
        for (int i = 0; i < n; i++) {
            vector<int> cnt(size);
            int maxfreq = 0;
            for (int j = i; j < n; j++) {
                cnt[s[j] - 'a']++;
                maxfreq = max(maxfreq,cnt[s[j] - 'a']);
                int minfreq = n;
                for (int k = 0; k < size; k++) {
                    if (cnt[k] > 0) {
                        minfreq = min(minfreq,cnt[k]);
                    }
                }
                res += maxfreq - minfreq;
            }
        }
        return res;
    }
};
```