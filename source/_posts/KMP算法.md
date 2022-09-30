# KMP算法的前提我们首先需要定义的是next数组的含义是什么；

- 1. next[j]表示的是，j位置及其以前的子串的最长相等前后缀的长度；
- 2. next[j]表示的是，j位置以前的子串的最长相等前后缀的长度；

## 情况1：包含j位置本身的情况

### 1.1：不统一减1的情况；next数组保存的是，包含当前字符的最长前后缀长度，这个长度的值可以作为数组下标，也是用来比较主串和模式串是否相等的位置；

```java
class Solution {
    // 前缀表不减一，直接作为next数组解答！
    public int strStr(String haystack, String needle) {
        if (needle.length() == 0) return 0;

        int[] next = new int[needle.length()];
        getNext(next, needle);


        for (int i = 0, j = 0; i < haystack.length(); ++i) {
            // 如果i和j不相等，就去找前一位的最大前后缀；
            while (j > 0 && haystack.charAt(i) != needle.charAt(j)) {
                j = next[j - 1];
            }
            // 相等了，i 和 j 都后移；
            if (haystack.charAt(i) == needle.charAt(j)) {
                ++j;
            }
            // 一旦超过t长度了，就代表匹配到了，此时i还没有加一，还位于最后一个匹配位置处！
            if (j == needle.length()) { // 注意这个位置
                return i - needle.length() + 1;
            }
        }
        return -1;
    }

    // 求next数组就是求最长相等前后缀长度
    public void getNext(int[] next, String pattern) {
        // next[j] 表示的是 j 位置（包含j）所保存最大相等前后缀的长度，这个长度直接作为数组标，表示的就是最长前缀的后一个位置
        // 也就是和主串比较的位置；
        char[] pat = pattern.toCharArray();
        // Next数组的回溯终点就是0-index:
        int j = 0;
        next[0] = j; // 0号位，只有一个字母，不存在最大前后缀，所以长度为0;
        for (int i = 1; i < pat.length; ++i) { // 对Next数组每个值进行归纳求解；
        // 找到与当前i相等的j位置，否则，当前位置不相等，那就找前一个位置的最长前后缀，
        // 反正就是找到一个位置的前后缀的后一个位置与i位置相等的位置，那么就在这个位置的基础上直接加一，就是所要的值；
            while (j > 0 && pat[j] != pat[i]) { // 找到一个相等的位置，或者初始位置；
                j = next[j - 1];
            }
            // 要么j为0，要么找到相等的位置；
            if (pat[j] == pat[i]) { // 找到相等的前后缀了；
            ++j; // 最长前后缀的长度加一，j 和 i同时后移，i后移在循环中实现；
            }
            // 当前i位置的要跳转的j位置就是，目前的j,就是最长相等前后缀的后面一个位置，也是包含i位置的，i处最长公共前后缀的长度；
            next[i] = j;
        }
    }
}
```

### 1.2 统一减去一的情况

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if (needle.length() == 0) return 0;
        char[] main = haystack.toCharArray();
        char[] model = needle.toCharArray();
        int[] next = new int[model.length];
        getNext(next, model);

        for (int i = 0, j = -1; i < main.length; ++i) {
            while (j >= 0 && main[i] != model[j + 1]) {
                j = next[j]; // 去上一个位置找
            }
            if (main[i] == model[j + 1]) {
                ++j; // 找到了就前进；
            }
            if (j == model.length - 1) { //注意这个位置；j经过加一后到达了model.length - 1,但i 位置还没有加一，始终在匹配串的末端；
                return i - model.length + 1;
            }
        }
        return -1;
    }


    public void getNext(int[] next, char[] pattern) {
        // 采用 “最大前后缀长度减一” 的方式初始化next数组；
        int j = -1;
        next[0] = j; // 初始化即-1了；
        for (int i = 1; i < pattern.length; ++i) {
            // 找到第一个相等的位置；
            while (j >= 0 && pattern[j + 1] != pattern[i]) { // 由于j 减去1了，所有就比较j + 1和i 的大小，和上个情况也是类似的；
                j = next[j]; // 去找前一个位置的最大前后缀；
            }
            if (pattern[j + 1] == pattern[i]) {
                j++; // 最大前后缀长度加一，i 和 j 同时移动；
            }
            next[i] = j; // 同样的，最长前后缀的长度作为数组下标，就是每个位置不匹配时，模式串需要和主串匹配的位置；
        }
    }
}
```

## 情况2：不包含j位置本身的next数组；

// 暂时没理清楚
