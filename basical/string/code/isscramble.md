题目描述：  
![image](/basical/string/image/image39.png)  
解题过程：  
一个很经典的回溯问题。  
一开始尝试用回溯的方法去解决这个问题，后来发现，因为要进行字符串的分割，再加上随机分割，我一度陷入了如何通过随机得到所有可能的字符串的问题中来，看了题解发现其实所谓的随机就是遍历所有的情况。回溯对于这个题是真的没办法啊，需要分割字符串然后重新拼接，而且还要对分割后的部分字符串继续重复一样的操作，那最终分割完所有的字符串后还要把这些字符串拼接起来，太难了。  
题解的算法：记忆化搜索：  
1. 在做题之前，定义若两个字符串互为对方的扰乱字符串则这两个字符串为和谐的
2. 定义函数f(i1,i2,length)，取值可能为true或者false，意为若s1从i1位置开始长度为length的子串和s2从i2位置开始长度为length的子串互为和谐的，则取值为true，否则为false。  
    其实在一开始这个函数的定义是：f(s1,s2)，但是在实际的代码实现过程中如果将字符串作为参数进行传递会占用很多空间导致空间复杂度比较高，因此用三个整型变量作为参数来减小空间的使用  
    在实际的代码实现过程中，函数f用记忆化搜索的方式来实现。首先定义一个memo数组，由于题目中的数据字符串s1和s2的长度最大为30，因此所有肯能取到的子串的位置为0~29，长度至多为30，因此memo数组初始化定义为memo[30][30][31]。而之所以使用记忆化搜索的方式**是因为记忆化搜索具有可以自顶向下解决规模逐渐减小的问题的特点**，如果使用动态规划必须从长度较小的字符串逐渐往长度大的字符串进行状态转移，而记忆化搜索可以直接从字符串s1开始向下分割，是不是很酷  
    memo数组的可能取值设定为-1，0，1，分别代表false、没有判断过、true  
3. 自顶向下分割字符串s1的过程：
    1. 使用函数dfs(int i1,int i2,int length)进行对字符串s1的自顶向下分割，起初调用为dfs(0,0,s1.length())，返回值为bool类型
    2. 如果memo[i1][i2][length]值为1，说明在这个位置上s1和s2是和谐的，直接返回true
    3. 如果s1和s2在这个位置上的子串相等，说明是和谐的，对memo数组进行赋值为1操作，返回true
    4. 如果s1和s2在这个位置上的字符串的某个字符出现的频率不相同则说明一定不是和谐的，对memo赋值为-1操作，返回false  
        在这里还多些了一个check函数，是对判断两个变量的某个更小的变量出现频次是否相同的一种非常经典的写法了，海涌到了algorithm库里的any_of函数，使用了lambda泛化表达式，很酷  
    5. 进行for循环操作，i的取值范围为1到length-1，意为在长度为1到长度为length-1的位置进行字符串分割，而这么对i取值的原因是不允许分割后出现空字符串  
        这个时候会有两种情况：  
        第一种：在i位置上分割后两部分字符串不发生交换，则如果s1的前半部分和s2对应的前半部分以及s1的后半部分和s2对应的后半部分分别都是和谐的，则说明s1和s2所对应的子串是和谐的，对memo进行赋值为1操作并且返回true  
        第二种：在i位置上分割后两部分字符串发生交换，则如果s1的后半部分和s2对应的前半部分以及s1的前半部分和s2对应的后半部分分别都是和谐的，则说明s1和s2所对应的子串是和谐的，对memo进行赋值为1操作并且返回true  
        而第一种情况和第二种情况在判断子串的某一个部分是否和谐时，其实调用了dfs函数，这就实现了自顶向下分割的操作。有一点很绝的是，一开始我拿到这个题目的时候思想一直是对原字符串s1的所有分割可能进行列举然后直接拿来和s2进行比较，在这个比较的过程中s1是会改变的，因为这是回溯的思想，然而记忆化搜索的过程根本不改变s1，只是不断地自顶向下地探讨每一种可能，而且是局部的比较不是整个拿来和s2进行比较，非常酷！！！  
    6. 退出for循环之后说明s1和s2对应部分的子串不是和谐的，对memo进行赋值为-1操作然后返回false即可  
以上所说的在dfs函数里面的s1和s2对应部分的子串都是s1和s2分别在i1位置和i2位置进行取长度为length的子串之后的操作得到的子串  
代码：（记忆化搜索）  
```cpp
class Solution {
private:
    int memo[30][30][31];
    string s1,s2;
public:
    bool checkSimilar(int i1,int i2,int length) {
        unordered_map<char,int> freq;
        for (int i = i1; i < i1 + length; i++) {
            ++freq[s1[i]];
        }
        for (int i = i2; i < i2 + length; i++) {
            --freq[s2[i]];
        }
        if (any_of(freq.begin(),freq.end(),[](const auto& entry){return entry.second != 0;})) {
            return false;
        }
        return true;
    }

    bool dfs(int i1,int i2,int length) {
        if (memo[i1][i2][length]) {
            return memo[i1][i2][length] == 1;
        }
        if (s1.substr(i1,length) == s2.substr(i2,length)) {
            memo[i1][i2][length] = 1;
            return true;
        }
        if (!checkSimilar(i1,i2,length)) {
            memo[i1][i2][length] = -1;
            return false;
        }
        for (int i = 1; i < length; i++) {
            if (dfs(i1,i2,i) && dfs(i1+i,i2+i,length-i)) {
                memo[i1][i2][length] = 1;
                return true;
            }
            if (dfs(i1+i,i2,length-i) && dfs(i1,i2+length-i,i)) {
                memo[i1][i2][length] = 1;
                return true;
            }
        }
        memo[i1][i2][length] = -1;
        return false;
    }

    bool isScramble(string s1, string s2) {
        memset(memo,0,sizeof(memo));
        this->s1 = s1;
        this->s2 = s2;
        return dfs(0,0,s1.length());
    }
};
```