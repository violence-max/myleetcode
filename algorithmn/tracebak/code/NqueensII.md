题目描述：  
![image](/algorithmn/tracebak/image/image17.png)  
解题过程： 
这道题忘记怎么写了，最终没有写出来，但是现在印象很深刻了，而且感觉回溯的题目的感觉又回来了。 
一开始写的时候记错了，以为每一行选一个，选中了之后把二维的判断数组的对应的列的剩余部分以及左下角和右下角的两个位置设置为占有就可以了。不过一开始还把左下角的赋值运算写错了，本来应该加一行，写成减一行。后面测试发现在回溯返回的过程中如果直接把左右下角的两个位置设置为可以放置会出现**覆盖**的现象—在第N+1个节点回溯返回的时候其左右下角的位置被设置成不可放置有可能是第N-1个节点造成的。于是我又加了个left和right的bool变量用来标识某一节点究竟有没有设置其左右下角节点为不可放置，但是，最让我担心的情况还是出现了—我一直以为逐行往下放置的过程不用砍掉一整个斜线，因此只设置了左右下角，但是在n=5的时候出现了被放置了的两个皇后通过斜线攻击的情况。然而我又觉得如果砍掉一整个斜线在回溯返回的过程中又会发生覆盖的情况而且也不能设置那么多标志变量，于是看了答案。 
题解很精髓，三个集合，一个列集合，一个左斜线集合还有一个右斜线集合。这三个集合都提取了关键特征：唯一性。每一个节点被设置成不可放置时其斜线的唯一性是不可覆盖的，就不会发生回溯返回时出错的问题了。  
右斜线的唯一性是指行坐标减去列坐标的值相等，左斜线是指行坐标加上列坐标的值相等。  
代码：  
```cpp
class Solution {
private:
    int ans = 0;
    int count = 0;
    unordered_set<int> col,dig1,dig2;
public:
    void (dfs(int n,vector<vector<bool>> &place,int count)) {
        if (count == n) {
            ans++;
            return;
        }
        for (int i = 0; i < n; i++) {
            if (place[count][i]) {
                if (col.find(i) != col.end()) continue;
                int digindex1 = count - i;
                if (dig1.find(digindex1) != dig1.end()) continue;
                int digindex2 = count + i;
                if (dig2.find(digindex2) != dig2.end()) continue;
                col.insert(i);
                dig1.insert(digindex1);
                dig2.insert(digindex2);
                dfs(n,place,count+1);
                col.erase(i);
                dig1.erase(digindex1);
                dig2.erase(digindex2);
            }
        }
    }
    int totalNQueens(int n) {
        vector<vector<bool>> place (n,vector<bool> (n,true));
        dfs(n,place,count);
        return ans;
    } 
};
```