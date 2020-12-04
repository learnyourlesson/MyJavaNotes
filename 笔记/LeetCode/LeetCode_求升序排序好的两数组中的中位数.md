**1.使用中位数的定义**

把一个集合分为两个，左右个数相同，右边集合中的任意一个数都大于左边集合中的任意一个数,这时，i+j=(n+m+1)/2(把奇数情况中位数分在左边)(j=(n+m+1)/2-i)，B[j-1]<=A[i]&&A[i-1]<=B[j]）当0<i<m时0<j<n,成立

i<m且m<n，j>(2m+1)/2-m>0,i>0&m<n&i∈N,j<(2n+1)/2-1<n

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        if (m > n) {
            int[] temps = nums1;
            nums1 = nums2;
            nums2 = temps;
            int temp = m;
            m = n;
            n = temp;
        }
        int min = 0, max = m, l = (m + n + 1) / 2;

        int j;
        while (min <= max) {
            int i = (min + max) / 2;
            j = l - i;
            if (i < max && nums2[j - 1] > nums1[i]) {
                min = i + 1;
            } else if (i > min && nums1[i - 1] > nums2[j]) {
                max = i - 1;
            } else {
                int maxLef;
                if (i == 0) {
                    maxLef = nums2[j - 1];
                } else if (j == 0) {
                    maxLef = nums1[i - 1];
                } else {
                    maxLef = Math.max(nums1[i - 1], nums2[j - 1]);
                }
                if ((n + m) % 2 == 1) {
                    return maxLef;
                }
                int minRig;
                if (i == m) {
                    minRig = nums2[j];
                } else if (j == n) {
                    minRig = nums1[i];
                } else {
                    minRig = Math.min(nums1[i], nums2[j]);
                }
                return (maxLef + minRig) / 2.0;
            }
        }
        return 0.0;
    }
}
```

用通俗的语言讲:就是把两个数组用i，j分为四块，两块属于中位数的左边，两块属于中位数的右边，i，j就是中位数的两个求和值（集合个数偶数情况），求得时候n+m进行+1把奇数的情况的中位数放在i，j分割的四快中的左边。如果A[i-1]>B[j]代表A的左边大于B的右边部分，所以要减小i的值，如果A[i]<B[j-1]就代表B的左边大于了A的右边，所以要减小j的值，即增大i的值。如果i，j其中一个在头部或者尾部的话，就简单了许多，i在A的尾部，A的元素全在中位数的左边，求Max(A[i-1],B[j-1])(左边最大的数)和B[j]的和的1/2就是中位数（偶数情况）奇数情况因为我们之前求j时，使用的是i+j=(n+m+1)/2,中位数必定在左边，所以求Max(A[i-1],B[j-1])(左边最大的数)即可，其他情况也类似

时间复杂度：O(log(min(m,n)))，

首先，查找的区间是 [0, m]。

而该区间的长度在每次循环之后都会减少为原来的一半。

所以，我们只需要执行 log(m) 次循环。由于我们在每次循环中进行常量次数的操作，所以时间复杂度为O(log(m))。

由于 m<n，所以时间复杂度是 O(log(min(m,n)))。

空间复杂度：O(1)，

我们只需要恒定的内存来存储 9 个局部变量， 所以空间复杂度为 O(1)。

**2.使用求第k项值的方式**

比较两个数组中的第K/2(向下取整了)个值，小的那个，及其前面的数都是不要的，因为K/2*2-1（1是大的那个数）=k-1,所以他不可能是第K个值，去掉K-小的那边去掉的个数 = new K ，重新计算

```java
class Solution {
    
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    int n = nums1.length;
    int m = nums2.length;
    int left = (n + m + 1) / 2;
    int right = (n + m + 2) / 2;
    //将偶数和奇数的情况合并，如果是奇数，会求两次同样的 k 。
    return (fun(nums1,nums2,0,0,left)+fun(nums1,nums2,0,0,right)) * 0.5;  
}
    
    public int fun(int[] nums1, int[] nums2, int i, int j, int k) {

        // if (nums1.length-i > nums2.length-j) {
        //     int[] temps = nums1;
        //     nums1 = nums2;
        //     nums2 = temps;
        //     int temp = i;
        //     i = j;
        //     j = temp;
        // }
        if (i >= nums1.length) {
            return nums2[j + k - 1];
        }
        if (j >= nums2.length) {
            return nums1[i + k - 1];
        }
        if (k == 1) {
            return Math.min(nums1[i], nums2[j]);
        }

       int oneHalf_K = k / 2;
//         int ii = i + Math.min(nums1.length-1-i+1, k / 2) - 1;
//         int jj = j + Math.min(nums2.length-1-j+1, k / 2) - 1;
       int ii = Math.min(nums1.length-1, (i + oneHalf_K - 1));
       int jj = Math.min(nums2.length-1, (j + oneHalf_K - 1));
        if (nums1[ii] < nums2[jj]) {
            return fun(nums1, nums2, ii + 1, j, k - (ii - i + 1));
        } else {
            return fun(nums1, nums2, i, jj + 1, k - (jj - j + 1));
        }
    }

}
```

时间复杂度：每进行一次循环，我们就减少 k/2 个元素，所以时间复杂度是 O(log(k)，而 k=(m+n)/2，所以最终的复杂也就是 O(log(m+n))。

空间复杂度：虽然我们用到了递归，但是可以看到这个递归属于尾递归，所以编译器不需要不停地堆栈，所以空间复杂度为 O(1)

借鉴：https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xun-zhao-liang-ge-you-xu-shu-zu-de-zhong-wei-shu-b/

https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-2/