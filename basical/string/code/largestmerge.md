题目描述：  
![image](/basical/string/image/image28.png)  
解题过程：  
写了一个半小时，操了。一开始想的是类似归并排序一样，然后直接出了一堆问题，因为当两个字符相等的时候不可以直接随意添加某一个字符，**而是应该看哪个字符的后缀字符串的字典序更大才添加哪个**。这个我还是看了题解才知道的，在知道这个之前发生了两次错误的尝试。我以为可以通过比较子串的最后一个字符的字典序来进行添加，**然而应该一个字符一个字符的添加**。我没有想到，字符串竟然可以直接比较字典序，太离谱了，我还手写了一个比较字符串字典序的函数，一个一个字符的比较，并且运用了递归的手法，干。  
代码：  
```cpp
class Solution {
public:
    bool check(string word1,string word2) {
        int ls1 = (int)word1.size(),ls2 = (int)word2.size();
        if ((ls1 == 0 && ls2 > 0) || (ls1 == 0 && ls2 == 0)) return false;
        else if (ls1 > 0 && ls2 == 0) return true;
        int start1 = 0,start2 = 0;
        while (start1 < ls1 && start2 < ls2) {
            if (word1[start1] == word2[start2]) {
                start1++; start2++;
            } else if (word1[start1] > word2[start2]) return true;
            else return false;
        }
        if (start1 < ls1) {
            return (check(word1.substr(0,start1),word1.substr(start1,ls1-start1))) ? false : true;
        } else if (start2 < ls2) {
            return (check(word2.substr(0,start2),word2.substr(start2,ls2-start2))) ? true : false;
        } else {
            return false;
        }
    }
    string largestMerge(string word1, string word2) {
        string merge;
        int ls1 = word1.size(),ls2 = word2.size();
        int index1 = 0,index2 = 0;
        while (index1 < ls1 && index2 < ls2) {
            if (word1[index1] < word2[index2]) {
                merge += word2[index2++];
            } else if (word1[index1] > word2[index2]){
                merge += word1[index1++];
            } else {
                if (check(word1.substr(index1+1,ls1-index1-1),word2.substr(index2+1,ls2-index2-1)))
                    merge += word1[index1++];
                else merge += word2[index2++];
            }
        }
        if (index1 == ls1 && index2 < ls2) merge += word2.substr(index2,ls2-index2);
        else if (index1 < ls1 && index2 == ls2) merge += word1.substr(index1,ls1-index1);
        return merge;
    }
};
```  
简洁版：  
```cpp
class Solution {
public:
    string largestMerge(string word1, string word2) {
        string merge;
        int ls1 = word1.size(),ls2 = word2.size();
        int index1 = 0,index2 = 0;
        while (index1 < ls1 || index2 < ls2) {
            if (index1 < ls1 && word1.substr(index1) > word2.substr(index2)) {
                merge += word1[index1++];
            } else {
                merge += word2[index2++];
            }
        }
        return merge;
    }
};
```