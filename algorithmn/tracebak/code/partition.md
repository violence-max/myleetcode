题目描述：  
![image](/algorithmn/tracebak/image/image6.png)  
解题过程：  
泪目，写了好久才写出来，两个小时吧差不多。  
算是对回溯又有一定的了解了。  
一开始以为可以用动态规划做，检测动态规划二维数组，对于true的部分直接取子串然后添加到答案里面。然而，居然是分割字符串。我直接看错题目！！！  
然后想用回溯去做，写了一个check函数，用来检测字符串是否为回文串。但是一开始的思路极其模糊，我都不知道自己在想什么了。  
直接说好的思路吧：  
深度遍历。从第一个节点开始，纵向递归遍历，横向循环遍历。当需要的字符串长度为0时返回。每个函数里面定义一个字符串，选择加入或者不加入，如果字符串为回文则寻找剩下的部分，如果不是则继续加入字符直到循环结束。  
无敌  
代码：  
```cpp
class Solution {
private:
    vector<vector<string>> ans;
    vector<string> temp;
    int length;
public:
    void dfs(string s,int index,int len) {
        if (len == 0) {
            ans.push_back(temp);
            return;
        }
        string str;
        for (int i = index; i < length; i++) {
            str.push_back(s[i]);
            if (check(str)) {
                temp.push_back(str);
                dfs(s,i+1,len-str.size());
                temp.pop_back();
            }
        }
    }

    bool check(string str) {
        int len = str.size();
        if (len < 2) return true;
        int left = 0,right = len - 1;
        while (right > left) {
            if (str[right--] != str[left++])
                return false;
        }
        return true;
    }

    vector<vector<string>> partition(string s) {
        length = s.size();
        if (length == 0) return {};
        dfs(s,0,length);
        return ans;
    }
};
```