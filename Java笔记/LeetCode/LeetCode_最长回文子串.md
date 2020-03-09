**1.中心扩展**

回文字符串关于中心对称，奇数是中心的char 偶数是 两个char中间的位置，

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s.length()<1||s==null) return "";
        int len1 = 0;
        int len2 = 0;
        int maxlen = 0;
        int start = 0;
        int end = 0;
        for (int i = 0; i < s.length(); i++) {
            // len1 = 0;
            // len2 = 0;
            //奇数情况
            len1 = fun(s, i, i);
            //偶数情况
            len2 = fun(s, i, i + 1);

            maxlen = Math.max(len1, len2);
            //求开始和结束位置
            if (maxlen > end - start) {
                start = i - (maxlen-1) / 2;
                end = i + maxlen / 2;
            }

        }
        return s.substring(start, end+1);
    }
//求回文子串长度
    public int fun(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left-1;
    }

}
```

注意：return s.substring()不能取到最后一个，所以end要加1，

return 子串长度时，left和right是最后错误的位置，和正确的位置前面少了一个，后面多了一个，多了两个，但直接减实际上是少算了一个的（没算开始那个：比如0-4[0,1,2,3,4]是5个数，但4-0=4），所以实际结果上只多了一个，所以减1

时间复杂度：*O*(*n*²)

空间复杂度：*O*(1)

**2.动态规划**

为了改进暴力法，我们首先观察如何避免在验证回文时进行不必要的重复计算。考虑“ababa” 这个示例。如果我们已经知道 “bab” 是回文，那么很明显，“ababa” 一定是回文，因为它的左首字母和右尾字母是相同的。

我们给出 P(i,j)P(i,j) 的定义如下：

![img](img/LeetCode_%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2/LeetCode_%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B21.png)

因此

![img](img/LeetCode_%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2/LeetCode_%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B22.png)

基本示例

![img](img/LeetCode_%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2/LeetCode_%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B23.png)

这产生了一个直观的动态规划解法，我们首先初始化一字母和二字母的回文，然后找到所有三字母回文，并依此类推…

复杂度分析

时间复杂度：*O*(*n*²)，这里给出我们的运行时间复杂度为*O*(*n*²))

空间复杂度：*O*(*n*²)，该方法使用 *O*(*n*²)的空间来存储表。

```java
public static String longestPalindrome(String s) {
        int n = s.length();
        String result = "";
        boolean[][] b = new boolean[n][n];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s.charAt(i) == s.charAt(j) && (j - i <= 1 || b[i + 1][j - 1])) {//这个写法的精髓就在这个判断
                    b[i][j] = true;
                    if (j - i >= result.length()-1) {//result.length()不-1求得就是最右边最长的那个回文子串，如：ababc 得 bab，-1后得aba
                        result = s.substring(i, j + 1);
                    }
                }

            }
        }
        return result;
    }
```

**3.Manacher 算法**

首先扩展原字符串，如 “aba”->"#a#b#a#",使字符串个数必定为奇数个，（偶数的中间个数为奇数，奇数+偶数=奇数）

然后申请一个辅助计算的数组，记录以新字符串中每个 元素用中心扩展法求得最大回文子串的信息（此处记录的是扩散步数，有些资料将辅助数组 p 定义为回文半径数组，即 p[i] 记录了以新字符串第 i 个字符为中心的回文字符串的半径（包括第 i 个字符），与我们这里定义的辅助数组 p 有一个字符的偏差，本质上是一样的）

此时其实已经可以计算最长回文子串了，但其实就是中心扩展法，所以优化之，

记录已经求出的当前已遍历的最右回文子串的对称中心（center）（新字符串必定是奇数，不用考虑在中心在两数中间的情况）及其向右扩展的最远边界（maxRight），

遍历新字符时，有三种情况

**1.p[mirror] 的数值比较小，不超过 maxRight - i(mirror是i关于center的对称点)**

p[i]=p[mirror]

**2.p[mirror] 的数值恰好等于 maxRight - i。**

首先p[i]=p[mirror]，然后继续尝试扩散

**3..p[mirror] > maxRight - i**

p[i] = maxRight - i，不可能再大（eabab(3)#b(2)ab(1)ac->#中心，最右的b为i，p[mirror]=3,e和c不可能相等（不然#为中心的回文子串会继续扩大）(e b 3) # (2 b c) e!=3所以2!=c）

**综上所述：i

时间复杂度：O(N)，由于 Manacher 算法只有在遇到还未匹配的位置时才进行匹配，已经匹配过的位置不再匹配，因此对于字符串 S 的每一个位置，都只进行一次匹配，算法的复杂度为 O(N)。

空间复杂度：O(N)。

Manacher（马拉车算法）其实是利用了一个辅助数组，根据辅助数组的辅助，实现了只对原数组的扩展版（为了合并奇数和偶数的情况）遍历一遍的中心扩展法。

```java
class Solution {
    public String longestPalindrome(String s) {

        //去掉一个长度和0个长度的（没有判空）
        if (s.length() < 2) return s;
        //使用流来进行字符串插入#
        String news = s.chars().mapToObj(c -> String.valueOf((char) c)).collect(Collectors.joining("#", "#", "#"));
        //新串长度
        int length = news.length();
        //辅助数组
        int[] p = new int[length];
        //当前遍历最右回文子串的中心及其最右边界
        int maxRight = 0;
        int center = 0;
        //记录结果的开始和位数
        int start = 0;
        int subLen = 1;
        for (int i = 0; i < length; i++) {
            if (i < maxRight) {
                int mirror = center * 2 - i;
                p[i] = Math.min(p[mirror], maxRight - i);
            }
            //i-+p[i]是当前位置对于p[i]处理后继续扩散的位置，+1是处理中心位置的问题
            int left = i - (p[i] + 1);
            int right = i + (p[i] + 1);
            while (left >= 0 && right < length && news.charAt(left) == news.charAt(right)) {
                p[i]++;
                left--;
                right++;
            }
            //right-1 = i+p[i] 根据 maxRight 的定义，它是遍历过的 i 的 i + p[i] 的最大者，就是当前最右回文子串的最右位置
            // 如果 maxRight 的值越大，进入上面 i < maxRight 的判断的可能性就越大，这样就可以重复利用之前判断过的回文信息了
            if (right - 1 > maxRight) {
                maxRight = right - 1;
                center = i;
            }

            if (p[i] > subLen) {
                // 记录最长回文子串的长度和相应它在原始字符串中的起点
                subLen = p[i];
                start = (i - subLen) / 2;
            }
        }

        return s.substring(start, start + subLen);
    
    }
}
```

借鉴：

https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode/

https://leetcode-cn.com/problems/longest-palindromic-substring/solution/dong-tai-gui-hua-by-a380922457/

https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zhong-xin-kuo-san-dong-tai-gui-hua-by-liweiwei1419/