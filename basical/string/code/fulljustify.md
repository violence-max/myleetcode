题目描述：  
![image](/basical/string/image/image35.png)
弱智一批，思路上完全就是一个简单题，实现上只是稍微有点花时间去想而已。  
算法流程：  
1. 用i进行字符串中单词的寻找，在while循环里每次用width（赋初值为0）记录已经找到的单词加空格组合起来的长度，start记录每次寻找时的开始下标。：  
    如果width加上下标i处的单词的长度没有超过maxWidth，则将该长度加到width上，如果width此时小于maxWidth（因为在实际添加的字符串中需要考虑单词之间的空格），则width加1  
2. 然后进行一个字符串的组成，即选中一定数量的单词后将其组成文本对齐的字符串并添加到答案中：
    1. 用last记录该字符串是否只具有一个单词或者是文本的最后一行，如果是赋值为true，否则为false。为了动态调整空格数量做铺垫
    2. 定义cnt记录空格的数量，赋初值为0
    3. 当start小于i时，将start处的单词添加到字符串末尾，若不是最后一个字符串则再添加一个空格
3. 动态调整字符串之间的空格：
    1. 如果该字符串只有一个单词或者为文本的末尾，则直接进行左对齐
    2. 用index作为索引，赋初值为0
    3. 当index小于该字符串的长度时进行while循环：
        1. 找到下一个空格的位置，如果找不到（说明遇到了该字符串的最后一个单词）则直接返回该字符串，因为调整已经成功了
        2. 计算需要在这个位置插入空格的数量insertNumber：用剩余的空格数量对还需要插入的空格的位置的个数（包括当前位置）进行相除后向上取整
        3. 在该位置之后插入空格。这里学到了字符串插入函数的写法：`str.insert(pos,number,c)`
        4. 索引index加上insertNumber再加1，来到下一个单词的开始的位置；字符串长度因为加入了insertNumber的长度发生了改变；需要插入的空格的位置的个数减1；需要插入的空格数减去insertNumber
整个题目每个部分都需要一定时间的思考，花时间最久的部分在动态调整字符串之间的空格上，写完了感觉这个算法很有趣。  
本来写了一半不想写了，太花时间了，然后看了题解，这写得什么玩意，根本看不下去，所以还是自己写一个吧。  
代码：  
```cpp
class Solution {
private:
    int n;
public:
    string formatString(vector<string> words,int start ,int i,int maxWidth) {
        string str;
        bool last = (i == n || i - start == 1) ? true : false;
        int cnt = 0;
        while (start < i) {
            str += words[start];
            if (start != i - 1) {
                str += ' ';
                cnt++;
            }
            start++;
        }
        return adjustString(str,maxWidth-(int)str.size(),last,cnt++);
    }

    string adjustString(string &str,int number,bool last,int cnt) {
        int ls = (int)str.size();
        if (last) {
            str.insert(ls,number,' ');
        } else {
            int index = 0;
            while (index < ls) {
                while (index < ls && str[index] != ' ')
                    index++;
                if (index == ls) return str;
                int insertNumber = (number + cnt - 1) / cnt;
                str.insert(index+1,insertNumber,' ');
                index += insertNumber + 1;
                ls += insertNumber;
                cnt--;
                number -= insertNumber;
            }
        }
        return str;
    }

    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        n = words.size();
        int i = 0;
        vector<string> ans;
        while (i < n) {
            int width = 0,start = i;
            while (i < n) {
                if (width + words[i].size() > maxWidth) break;
                width += words[i++].size();
                if (width < maxWidth) width += 1;
            }
            ans.push_back(formatString(words,start,i,maxWidth));
        }
        return ans;
    }
};
```