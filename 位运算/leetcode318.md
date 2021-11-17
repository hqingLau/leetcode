## [318. 最大单词长度乘积](https://leetcode-cn.com/problems/maximum-product-of-word-lengths/)

给定一个字符串数组 `words`，找到 `length(word[i]) * length(word[j])` 的最大值，并且这两个单词不含有公共字母。你可以认为每个单词只包含小写字母。如果不存在这样的两个单词，返回 0。

**示例 1:**

```
输入: ["abcw","baz","foo","bar","xtfn","abcdef"]
输出: 16 
解释: 这两个单词为 "abcw", "xtfn"。
```

**示例 2:**

```
输入: ["a","ab","abc","d","cd","bcd","abcd"]
输出: 4 
解释: 这两个单词为 "ab", "cd"。
```

**示例 3:**

```
输入: ["a","aa","aaa","aaaa"]
输出: 0 
解释: 不存在这样的两个单词。
```

 

**提示：**

- `2 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- `words[i]` 仅包含小写字母

## 题解

### 一、set精简

将字符串经过`set`精简后用于后续的比较。

```java
class Solution {
    public int maxProduct(String[] words) {
        Set[] chars = new Set[words.length];
        for(int i=0;i<words.length;i++) {
            chars[i] = new HashSet<String>();
            Collections.addAll(chars[i],words[i].split(""));
        } 

        int result = 0;
        for(int i=0;i<words.length;i++) {
            for(int j=i+1;j<words.length;j++) {
                // 不相交，没有相同元素
                if(Collections.disjoint(chars[i],chars[j])) {
                    result = Math.max(result,words[i].length()*words[j].length());
                }
            }
        }
        return result;
    }
}
```

- 时间复杂度：$O(n^2+set的操作)$，精简后字符串为常数级（不超过26），比较复杂度为常数。

### 二、位图

一个`word`的有用信息就是长度以及存在哪些字符。

长度可以从`words`数组中得到。

存在哪些字符不需要`set`，一个`int`类型就满足了，也就是位图形式，效率较高。

代码：

```java
class Solution {
    public int maxProduct(String[] words) {
        int[] bitWord = new int[words.length];
        for(int i=0;i<words.length;i++) {
            for(int j=0;j<words[i].length();j++) {
                bitWord[i] |= 1<<words[i].charAt(j)-'a';
            }
        } 

        int result = 0;
        for(int i=0;i<words.length;i++) {
            for(int j=i+1;j<words.length;j++) {
                if((bitWord[i]&bitWord[j])==0) {
                    result = Math.max(result,words[i].length()*words[j].length());
                }
            }
        }
        return result;
    }
}
```

- 时间复杂度：$O(n^2+位图的操作)$。其中字符串的比较用的是`&`符号，效率比`set`查交集要高。也就是这里的$n^2$系数比`set`的$n^2$系数要小。

