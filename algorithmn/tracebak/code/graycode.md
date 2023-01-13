题目描述：  
![image](/algorithmn/tracebak/image/image19.png)  
解决过程：  
没有找到格雷码的规律，然后开始尝试暴力回溯。主要思想是答案容器中的第一个数据是0，根据输入的n得到最终答案容器中的最大数据为height = 2^n - 1，就可以根据height计算出答案容器中的每个数据应该按照二进制中一共有heightBit = log(height)来进行计算，于是，我们就可以每次从答案数组中取出最后的那个数据last，从低位到高位进行循环，尝试得到每一位取反但是其余位不动的数据next，具体做法是：next = last ^ (1 << i)，i为遍历到的位数（从0开始），然后判断next是否已经加入答案容器中，如果没有就加入然后开始下一个数据的判断  
**对称翻转：**  
这个方法很秀，采取的是很tricky的观察规律的方式。  
我们假设G(n-1)代表整数n-1所求出的正确的格雷码序列，而对于这一组格雷码序列而言，我们假设其分别为a0,a1,a2,……,an_2,an_1,an，那么我们在求G(n)的时候，假设这n+1个数据可以作为G(n)序列的前缀部分，根据格雷码的定义规则，G(n)序列的数据个数刚好是G(n-1)的两倍，因此对于G(n-1)的每个数据，我们将其前缀加0，相当于是扩充了一位，例如：00→000。对于G(n)的后n+1位，我们就可以想到一种很自然的方式使其满足格雷码的规则：对于G(n-1)将其翻转到G(n)的剩余的后n+1个位置上，再将其前缀由0改变为1，这时，G(n)的前后两个部分都满足了格雷码规则，而且第一个元素和最后一个元素仅仅是在最高位不同也满足规则，中间衔接的部分也仅仅是最高位不同也满足规则，就得到了正确的G(n)  
然而，代码实现的过程稍微有点tricky：  
题目要求求出n=1到n=16的格雷码序列，然而为了得到n=1的格雷码序列，根据推到规则我们必须要先得到n=0的格雷码序列。当n=0时，其格雷码序列个数应为2^0=1个，又因为格雷码序列的第一个整数为0，因此G(0) = {0}  
起初，我们定义一个答案容器ret。很容易知道，ret的大小应该为2^n，然而，如果提前定义ret的大小就只能通过额外定义一个整型变量来充当索引对ret进行赋值，**不能使用push_back()方法，因为push_back()函数会使得vector容器的大小发生改变**。而如果不提前定义ret的大小，每次要新增元素时都使用push_back()方法，那效率会非常低，因为push_back()方法有可能需要连续的开辟空间，而vector容器的开辟空间的原理是复杂的“寻找空间、移动数据、释放原空间”三部曲，很耗时。因此，可以使用reserve函数提前预留出2^n大小的空间，每次新增元素时使用push_back()方法或者提前定义好reserve的大小额外使用一个整型变量记录应该赋值的索引的位置。  
因为n=0时的格雷码序列可以不求自出，为了得到n的格雷码序列，根据推导规则，我们需要从i=1到i=n进行循环，逐步求出i的格雷码序列并且添加到ret中  
每次进行求解的时候的规则就比较简单了，对于已经求出的序列我们从后往前进行遍历就模拟了对称翻转的过程，对于每一个遍历到的值，我们只需要在其前缀加1，相当于加上(1 << (i-1))，即可。对于这里的解释，是：对于每个i的求解，我们已经求出了i-1的解，因此已经得到的格雷码序列的每个数据都一共有i-1位，然而这个i-1位是从1开始计数的。在添加前缀时，我们刚好是需要在第i-1位添加一个1  
**公式：**  
公式的证明是最有意思的部分，为了使证明看上去很轻松，我将采用最通俗易懂的自顶向下解读题目的手法  
对于n的格雷码的求解，其格雷码序列一共有2^n个数据，分别为0到2^n-1。那么，我们对这些数据使用代数表示，分别为b0，b1，……，b2^n-1。对于其相邻的两个数据，我们假设分别是bi和bi+1，显然bi+1 = bi + 1，因此，bi+1和bi的区别就是末位的连续的1变为0，最低位的0变成1（二进制加法）。因为一定有一种算法让我们可以对bi进行一定的变化得到其对应的格雷码，而且满足格雷码规则，这是一种一对一映射的关系（这里根据已经存在的格雷码规则进行说明，不是很严谨）。我们观察bi+1和bi的区别，假设对于bi而言，加1以后在第k位发生由0到1的突变，则第0位到第k-1位发生1到0的改变。因为0和1与0异或的结果为本身，与1异或的结果为取非得值，所以根据bi+1和bi的第k位的不同来作文章，我们只需要对于bi和bi+1的第k位到第n位都分别异或相同的数，那么这部分数异或出来得到的结果就刚好在第k位有一个数据不相同。而bi的第0位到第k-1位数据都为1，bi+1的第0位到第k-1位的数据都为0，如果这两个部分分别异或0……111以及1……000就可以得到完全相同的01串：1……000。综合两个部分，可以发现，对于bi = bn-1……bkbk_1……b2b1b0而言，我们假设bn自动补0，如果bi按位与bn……bk+1bk……b3b2b1进行异或，刚好会在第k位不同。这是我个人对公式的解读  
关于代码部分的ret[i] = (i >> 1) ^ i，就可以理解成bi右移一位后与自身按位异或得到bi+1  
代码：（回溯→对称翻转→公式）  
```cpp
class Solution {
private:
    vector<int> ans = {0};
    int height;
    int heightBit;
    int tmp;
    unordered_set<int> set = {0};
    bool found = false;
public:
    void dfs(int n) {
        if (n == height + 1) {
            found = true;
            return;
        }
        int last = ans[ans.size() - 1];
        for (int i = 0; i < heightBit; i++) {
            int bit = 1 << i;
            int next = last ^ bit;
            if (set.count(next)) {
                continue;
            } else {
                set.insert(next);
                ans.push_back(next);
                dfs(n+1);
                if (found) return;
                set.erase(next);
                ans.pop_back();
            }
        }
    }

    vector<int> grayCode(int n) {
        int height = pow(2,n) - 1,heightBit = 0;
        int tmp = height;
        while (tmp) {
            heightBit++;
            tmp >>= 1;
        }
        this->height = height;
        this->heightBit = heightBit;
        dfs(1);
        return ans;
    }
};
```
```cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> ret;
        ret.reserve(1 << n);
        ret.push_back(0);
        for (int i = 1; i <= n; i++) {
            int m = ret.size();
            for (int j = m - 1; j >= 0; j--) {
                ret.push_back(ret[j] + (1 << (i - 1)));
            }
        }
        return ret;
    }
};
```
```cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> ret (1 << n);
        for (int i = 0; i < ret.size(); i++) {
            ret[i] = (i >> 1) ^ i;
        }
        return ret;
    }
};
```