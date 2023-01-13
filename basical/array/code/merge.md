题目描述：  
![image](/basical/array/image/image37.png)  
解题过程：  
写这道题居然用了十几分钟，难受死。一开始想的是归并排序里的归并算法，但是居然没有想到用一个额外的数组来临时存放数据？？？居然还是想着在nums1进行原地插入，原地插入的同时又要修改nums1的长度，麻烦地要死，最要命的是插入会使得nums1的长度真的变长，然而题目的要求中nums1的长度是定死的。所以，我又换了一种写法，不插入了，每次应该插入时把nums1中该位置上所有数据往后挪一格。我以为这样子写的时间复杂度会很慢，没想到0ms通过，笑了。  
看了题解，直接排序法，归并排序法，从后向前遍历法，都挺不错的，我觉得从后向前遍历法是最好的，因为不需要额外的空间，也不会有什么最坏情况需要O(n^2)的时间复杂度的情况。  
代码：（我的→归并排序→从后向前遍历）  
```cpp
class Solution {
public:
    void transfer(vector<int> &nums,int first,int last) {
        for (int i = last - 1; i >= first; i--) {
            nums[i+1] = nums[i];
        }
    }

    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = 0,j = 0,last = m;
        while (i < last && j < n) {
            if (nums1[i] > nums2[j]) {
                transfer(nums1,i,last);
                last++;
                nums1[i] = nums2[j++];
            }
            i++;
        }
        if (j < n) {
            while (i < m + n) {
                nums1[i++] = nums2[j++];
            }
        }
    }
};
```
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int sorted[m+n];
        int i = 0,j = 0,k = 0;
        while (i < m || j < n) {
            int cur;
            if (i == m) {
                cur = nums2[j++];
            } else if (j == n){
                cur = nums1[i++];
            } else if (nums1[i] > nums2[j]) {
                cur = nums2[j++];
            } else cur = nums1[i++];
            sorted[k++] = cur;
        }
        for (int i = 0; i < m + n; i++) {
            nums1[i] = sorted[i];
        }
    }
};
```
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1,j = n - 1,tail = m + n - 1;
        while (i >= 0 || j >= 0) {
            int cur;
            if (i == -1) {
                cur = nums2[j--];
            } else if (j == -1) {
                cur = nums1[i--];
            } else if (nums1[i] < nums2[j]) {
                cur = nums2[j--];
            } else cur = nums1[i--];
            nums1[tail--] = cur;
        }
    }
};
```