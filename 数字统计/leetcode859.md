## [859. 亲密字符串](https://leetcode-cn.com/problems/buddy-strings/)

给你两个字符串 `s` 和 `goal` ，只要我们可以通过交换 `s` 中的两个字母得到与 `goal` 相等的结果，就返回 `true` ；否则返回 `false` 。

交换字母的定义是：取两个下标 `i` 和 `j` （下标从 `0` 开始）且满足 `i != j` ，接着交换 `s[i]` 和 `s[j]` 处的字符。

- 例如，在 `"abcd"` 中交换下标 `0` 和下标 `2` 的元素可以生成 `"cbad"` 。



**示例 1：**

```
输入：s = "ab", goal = "ba"
输出：true
解释：你可以交换 s[0] = 'a' 和 s[1] = 'b' 生成 "ba"，此时 s 和 goal 相等。
```

**示例 2：**

```
输入：s = "ab", goal = "ab"
输出：false
解释：你只能交换 s[0] = 'a' 和 s[1] = 'b' 生成 "ba"，此时 s 和 goal 不相等。
```

**示例 3：**

```
输入：s = "aa", goal = "aa"
输出：true
解释：你可以交换 s[0] = 'a' 和 s[1] = 'a' 生成 "aa"，此时 s 和 goal 相等。
```

**示例 4：**

```
输入：s = "aaaaaaabc", goal = "aaaaaaacb"
输出：true
```

 

**提示：**

- `1 <= s.length, goal.length <= 2 * 104`
- `s` 和 `goal` 由小写英文字母组成

## 题解

```java
class Solution {
    public boolean buddyStrings(String s, String goal) {
        if(s.length()<=1 || s.length()!=goal.length()) return false;
        int firstDiff = -1; //不同之处出现的位置
        int secondDiff = -1;
        for(int i=0;i<s.length();i++) {
            if(s.charAt(i)!=goal.charAt(i)) {
                if(firstDiff==-1) firstDiff=i;
                else if(secondDiff==-1) secondDiff=i;
                else return false;
            }
        }
        // 全相同，看有没有重复的，重复就返回true,没有就false
        if(firstDiff==-1) {
            int[] cnt = new int[26];
            for(int i=0;i<s.length();i++) {
                cnt[s.charAt(i)-'a']++;
                if( cnt[s.charAt(i)-'a']>=2) return true;
            }
            return false;
        }
        // 有一个字母不同，必错
        if(secondDiff==-1) return false;
        // 两个字母不同，看能否交换相等
        if(s.charAt(firstDiff)!=goal.charAt(secondDiff) || 
                goal.charAt(firstDiff)!=s.charAt(secondDiff)) {
            return false;
        }
        return true;
    }
}
```

