题目描述：  
![image](/basical/gragh/image/image1.png)  
解决过程：  
图论这个玩意根本就没有写过相关的题目，看到这个的第一眼完全就可以只能遍历或者循环或者回溯，再牛一点的就是记忆化搜索。  
看了题解，没有想到啊，居然学到了一招—并查集  
并查集真的是太帅了，主要思想是一个节点可能有很多孩子的同时也会有一个父亲，而在递归查询一个节点的时候总是会找到最根源的那个节点，如果两个节点的根源节点相同那么这两个节点就在同一个集合中，否咋不在。  
这道题就是依靠排序加并查集做出来的，包括排序的思想也很帅，把小的limit边先处理，然后再edgelist中就可以设置一个索引一直往下走，简直强无敌  
代码： 
```cpp
class Solution {
public:
    int find(vector<int> &uf,int x) {
        if (uf[x] == x) {
            return x;
        }
        return uf[x] = find(uf,uf[x]);
    }

    void merge(vector<int> &uf,int x,int y) {
        x = find(uf,x);
        y = find(uf,y);
        uf[y] = x;
    }

    vector<bool> distanceLimitedPathsExist(int n, vector<vector<int>>& edgeList, vector<vector<int>>& queries) {
        sort(edgeList.begin(),edgeList.end(),[](vector<int> u,vector<int> v) {
            return u[2]  < v[2];
        });
        int m = edgeList.size(),N = queries.size();
        vector<int> index (N);
        iota(index.begin(),index.end(),0);
        sort(index.begin(),index.end(),[&](int i,int j) {
            return queries[i][2] < queries[j][2];
        });
        int k = 0;
        vector<int> uf (n);
        iota(uf.begin(),uf.end(),0);
        vector<bool> res (N);
        for (auto i : index) {
            while (k < m && edgeList[k][2] < queries[i][2]) {
                merge(uf,edgeList[k][0],edgeList[k][1]);
                k++;
            }
            res[i] = find(uf,queries[i][0]) == find(uf,queries[i][1]);
        }
        return res;
    }
};
```