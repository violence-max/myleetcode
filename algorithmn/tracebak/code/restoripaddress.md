题目描述：  
![image](/algorithmn/tracebak/image/image7.png)  
解题过程：  
想用回溯的思路解决，就是选择当前值作为断点加入，但是在剪枝和回溯的逻辑上都没有做好。看了题解，有个大神的答案真的强无敌。  
从0索引开始，计数为0，当计数到达4时说明IP地址的四段区域都被分配好了，若此时索引值到达字符串尾部，则将该IP地址加入答案中。  
对三种长度进行循环，因为IP地址的每一段至多只有三位：  
若当前索引值加上对应长度超出字符串大小则退出；  
若当前索引值对应位置为字符0而待判断长度大于1则退出，0不能做前导字符；  
若待判断长度为3且从索引值开始的长度为3的子串大于字符串”255“，则退出，IP地址的每个部分的整数值在0~255之间（**判断字符串大小太骚了**）  
将从索引位置开始的待判断长度的子串加入temp字符串中，加入句点，计数增一，索引值加对应长度，开始下一次回溯  
回溯结束，将temp重新赋值，去掉句点和对应的子串  
代码：  
```cpp
class Solution {
private:
    vector<string> ans;
    string temp;
public:
    void dfs(string s,int index,int count) {
        if (count == 4 || index == s.size()) {
            if (count == 4 && index == s.size())
                ans.push_back(temp.substr(0,temp.size()-1));
            return;
        }
        for (int L = 1; L <= 3; L++) {
            if (index + L > s.size()) return;
            if (s[index] == '0' && L != 1) return;
            if (L == 3 && s.substr(index,L) > "255") return;
            temp += s.substr(index,L);
            temp.push_back('.');
            dfs(s,index+L,count+1);
            temp = temp.substr(0,temp.size()-L-1);
        }
    }
    vector<string> restoreIpAddresses(string s) {
        if (s.size() > 12 || s.size() < 4) return {};
        dfs(s,0,0);
        return ans;
    }
};
```