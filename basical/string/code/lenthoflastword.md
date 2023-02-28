题目描述：  
![image](/basical/string/image/image29.png)  
解决过程：  
常规思路，首先越过空格，然后用start记录一个单词的起始位置，当i来到空格时记录单词长度。但是一开始写成了记录最大的单词的长度了。。。后面有个思路，就是把起初的越过所有空格加入循环里来，就写成了先越过所有空格再对start赋值为i，然后当i前进到空格的位置时用i-start来记录单词的长度。但是这样会发生当字符串最后的字符是空格时长度为0的情况，于是添加了一条判断语句就可以了。  
看了题解，直接倒回来进行找单词然后记录长度，这个方法应该快很多。  
代码：  
```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
        int ls = (int)s.size();
        int i = 0,ans = 0;
        while (i < ls) {
            while (i < ls && s[i] == ' ')
                i++;
            int start = i;
            while (i < ls && s[i] != ' ')
                i++;
            ans = (i - start == 0) ? ans : i - start;
        }
        return ans;
    }
};
```  
题解：  
```cpp
class Solution {
    public int lengthOfLastWord(String s) {
        int index = s.length() - 1;
        while (s.charAt(index) == ' ') {
            index--;
        }
        int wordLength = 0;
        while (index >= 0 && s.charAt(index) != ' ') {
            wordLength++;
            index--;
        }
        return wordLength;
    }
}
```