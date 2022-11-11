题目描述：  
![image](/basical/string/image/image8.png)  
解题过程：  
这个题属于一看就会的类型，只不过需要一点点时间去写一下而已，思路真的很简单，就是把每个单词提取出来然后全部挪到另外一个字符串而已。我在做的时候老是忘记这个题目要求我们把单词给反转过来，于是就把这个题做成了去掉多余空格的题目。也没有比较tricky的地方吧，就是必须要注意以下题目的整体性和空格的跳过。要求用一个O(1)的空间复杂度去做的时候我觉得有点小难，题解给出了一种方法，就是先把整个字符串反转然后单独对每个单词做反转，也行吧。  
代码：  
```cpp
class Solution {
public:
    void pushString(string &s,string toBePushed,bool space) {
        for (char c : toBePushed) {
            s.push_back(c);
        }
        if (space) s.push_back(' ');
    }
    string reverseWords(string s) {
        string res;
        vector<string> container;
        int i = 0,length = (int)s.size(),count = 0;
        while (i < length && s[i] == ' ')
            ++i;
        while (i < length) {
            string temp;
            while (i < length && s[i] != ' ') {
                temp.push_back(s[i++]);
            }
            container.push_back(temp);
            ++count;
            while (i <length && s[i] == ' ')
                ++i;            
        }
        for (int j = count - 1; j >= 0; j--) {
            if (j == 0) pushString(res,container[j],false);
            else pushString(res,container[j],true);
        }
        return res;
```