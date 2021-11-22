## [384. 打乱数组](https://leetcode-cn.com/problems/shuffle-an-array/)

难度中等184

给你一个整数数组 nums ，设计算法来打乱一个没有重复元素的数组。

实现 `Solution` class:

- `Solution(int[] nums)` 使用整数数组 `nums` 初始化对象
- `int[] reset()` 重设数组到它的初始状态并返回
- `int[] shuffle()` 返回数组随机打乱后的结果

 

**示例：**

```
输入
["Solution", "shuffle", "reset", "shuffle"]
[[[1, 2, 3]], [], [], []]
输出
[null, [3, 1, 2], [1, 2, 3], [1, 3, 2]]

解释
Solution solution = new Solution([1, 2, 3]);
solution.shuffle();    // 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。例如，返回 [3, 1, 2]
solution.reset();      // 重设数组到它的初始状态 [1, 2, 3] 。返回 [1, 2, 3]
solution.shuffle();    // 随机返回数组 [1, 2, 3] 打乱后的结果。例如，返回 [1, 3, 2]
```

 

**提示：**

- `1 <= nums.length <= 200`
- `-106 <= nums[i] <= 106`
- `nums` 中的所有元素都是 **唯一的**
- 最多可以调用 `5 * 104` 次 `reset` 和 `shuffle`

## 题解

### Random

从后往前遍历，和前面的数或自己随机交换。

例如[1,3,5,**4**,2]中的4，从1,3,5,4选择一个进行交换，那最后的2呢？因为逆序遍历的时候，2已经给过机会和4交换了（也就是4有了一次机会和2交换）。这样，4其实有同等机会和所有元素交换，这就打乱了。

```java
class Solution {
    int[] nums;
    Random rand = new Random();
    public Solution(int[] nums) {
        this.nums = nums;
    }
    
    public int[] reset() {
        return nums;
    }
    
    public int[] shuffle() {
        int[] result = nums.clone(); //Arrays.copyOf(nums,nums.length);
        for(int i=nums.length-1;i>0;i--) {
            //生成一个随机的 int 值，该值介于 [0,i+1)
            swap(result,i,rand.nextInt(i+1));
        }
        return result;
    }
    private void swap(int[] arr,int i,int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
```

### shuffle(仅为熟悉函数使用)

```java
class Solution {
    int[] nums;
    public Solution(int[] nums) {
        this.nums = nums;
    }
    
    public int[] reset() {
        return nums;
    }
    
    public int[] shuffle() {
        List<Integer> list = Arrays.stream(nums).boxed().collect(Collectors.toList());
        Collections.shuffle(list);
        return list.stream().mapToInt(Integer::valueOf).toArray();
    }
}
```

