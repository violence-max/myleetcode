题目描述：  
![image](/basicaldatastructure/setandmap/image/image9.png)  
解决过程：  
没做出来，做了一个多小时  
- 哈希表：
1. 使用哈希表来构建字符串到最低的rank的索引，这里的rank指的是同一个字符出现的排位的下一个位置
2. 遍历names，如果不存在，则直接加入哈希表中，rank设为1；否则从rank开始寻找不在哈希表中的有效排位，找到之后，更新当前遍历字符的rank以及对于新得到字符串设置rank为1
3. 注意答案的加入  
代码：  
```cpp
class Solution {
public:
    string addsuffix(string name, int rank) {
        return name + '(' + to_string(rank) + ')';
    }

    vector<string> getFolderNames(vector<string>& names) {
        unordered_map<string,int> index;
        vector<string> ans (names.size());
        for (int i = 0; i < names.size(); i++) {
            if (!index.count(names[i])) {
                ans[i] = names[i];
                index[names[i]] = 1;
            } else {
                int rank = index[names[i]];
                while (index.count(addsuffix(names[i], rank))) {
                    rank++;
                }
                ans[i] = addsuffix(names[i], rank);
                index[addsuffix(names[i],rank)] = 1;
                index[names[i]] = rank;
            }
        }
        return ans;
    }
};
```