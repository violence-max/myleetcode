题目描述：  
![image](/basicaldatastructure/setandmap/image/image7.png)  
解决过程：  
想了十分钟，没想到一个合理的解决方案，看了题解，真是让我耳目一新。题解里面的命名做得很好，timeMap，一个非常优雅的名字。还有，对于时间字符串的处理，直接转换成分钟，逻辑是在是太清晰了  
代码：  
```cpp
class Solution {
public:
    vector<string> alertNames(vector<string>& keyName, vector<string>& keyTime) {
        int n = keyName.size();
        unordered_map<string,vector<int>> timeMap;
        for (int i = 0; i < n; i++) {
            string name = keyName[i];
            string time = keyTime[i];
            int hour = (time[0] - '0') * 10 + time[1] - '0';
            int minute = (time[3] - '0') * 10 + time[4] - '0';
            timeMap[name].emplace_back(hour * 60 + minute);
        }
        vector<string> ans;
        for (auto& [name,_list] : timeMap) {
            sort(_list.begin(),_list.end());
            for (int i = 2; i < _list.size(); i++) {
                int time_1 = _list[i-2], time_2 = _list[i];
                int difference = time_2 - time_1;
                if (difference <= 60) {
                    ans.emplace_back(name);
                    break;
                }
            }
        }
        sort(ans.begin(),ans.end());
        return ans;
    }
};
```