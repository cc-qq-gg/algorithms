# algorithms records of algorithms learning

伟人曾经说过，概念是知识的基石。

### 第一天

算法复杂度

> 1. 时间复杂度：指输入数据大小为 NN 时，算法运行*所需计算操作数量*
>    > 从小到大排序，常数，对数，线性，平方阶，指数，阶乘 O(1)<O(logN)<O(N)<O(NlogN)<O($N^2$)<O($2^N$)<O(N!)
>    > 分别对应，恒定输出，二分法，单轮 for 循环，两层循环独立，递归。
> 2. 空间复杂度：

两数之和

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target 的那 两个 整数，并返回它们的数组下标。

```js
// 官方map方案，约耗时60
var twoSum = function (nums, target) {
  const hashMap = {}
  for (const idx in nums) {
    const current = nums[idx]
    const tag = target - current
    const index = hashMap[tag]
    if (index) {
      return [index, idx]
    } else {
      hashMap[current] = idx
    }
  }
}
// 约耗时160ms
var twoSum = function (nums, target) {
  let result = []
  for (let idx = 0; idx < nums.length; idx++) {
    const index = nums.indexOf(target - nums[idx])
    if (index !== -1 && index !== idx) {
      result = [index, idx]
      break
    }
  }
  return result
}
```

经典二分查找

```js
var search = function (nums, target) {
  let low = 0
  let high = nums.length - 1
  while (low <= high) {
    const midIdx = Math.floor((high - low) / 2) + low
    const num = nums[midIdx]
    if (num === target) {
      return midIdx
    } else if (num > target) {
      high = midIdx - 1
    } else {
      low = midIdx + 1
    }
  }
  return -1
}
```

二分法的应用：第一个错误版本

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left = 1;
        int right = n;
        while(left < right) {
            int version = (right + left) / 2;
            if(isBadVersion(version)) {
               right = version; // 答案区间[left, version]
            } else {
                left = version + 1; // 答案区间[left + 1, version]
            }
        }
        return left;
    }
}
```

二分法应用：查找插入位置

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length -1;
        // 超出边界的情况
        if (target < nums[left]) return 0;
        if (target > nums[right]) return right + 1;

        while(left <= right) {
            int idx = (left + right) / 2;
            int num = nums[idx];
            if (target ==  num) {
                return idx;
            } else if (target > num) {
              // 不对
               left = idx + 1;
               // 判断下次是否为最后一次查找
               if (left == right) return right;
            } else {
              // 不对
                right = idx - 1;
               // 判断下次是否为最后一次查找
                if (left == right) return right;
            }
        }

       return 0;
    }
}
// 这种更简洁，巧妙
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length -1;
        // 超出边界的情况
        if (target < nums[left]) return 0;
        if (target > nums[right]) return right + 1;
        // left == right 时进行最后一次运算，
        // 倒数第二次运算时， left = right - 1
        while(left <= right) {
            int idx = (left + right) / 2;
            int num = nums[idx];

            if (target > num) {
               left = idx + 1;
            } else {
                right = idx - 1;
            }
        }
      // 只能返回left，巧妙
       return left;
    }
}
```

双指针查找：有序数组的平方

> 一个递增的整数数组，返回每个数字的平方组成的递增数组

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
       int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0, j = n - 1, pos = n - 1; i <= j; ) {
            int _i = nums[i] * nums[i];
            int _j = nums[j] * nums[j];
            if (_i > _j) {
                ans[pos] = _i;
                i++;
            } else {
                ans[pos] = _j;
                j--;
            }
            pos--;
        }
        return ans;
    }
}
```

旋转数组

> java 版，提交测试的时间消耗居然是 0

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int len = nums.length;
        k %= len; // 取余数，以适应超出数组长度的情况
        if (k == 0) return;
        int[] start = Arrays.copyOfRange(nums, len - k, len);
        int[] end = Arrays.copyOf(nums, len - k);
        // 拼接
        System.arraycopy(start, 0, nums, 0, start.length);
        System.arraycopy(end, 0, nums, start.length, end.length);
    }
}
```

> js 版，和 java 版内存消耗差不多，但耗时 100ms 左右

```js
var rotate = function (nums, k) {
  k = k % nums.length
  const temp = nums.slice(nums.length - k).concat(nums.slice(0, nums.length - k))
  nums.splice(0, nums.length, ...temp)
}

// 数组翻转
let reverse = function (nums, start, end) {
  while (start < end) {
    ;[nums[start++], nums[end--]] = [nums[end], nums[start]]
  }
}
let rotate = function (nums, k) {
  k %= nums.length
  reverse(nums, 0, nums.length - 1)
  reverse(nums, 0, k - 1)
  reverse(nums, k, nums.length - 1)
  return nums
}
```

移动零

```js
// 第一个版本，性能较低300ms以上，应该是数组方法的原因
var moveZeroes = function (nums) {
  const n = nums.length
  for (let i = 0, j = n - 1; i <= j; i++) {
    if (nums[i] === 0) {
      nums.splice(i, 1)
      nums.push(0)
      i--
      j++
    }
  }
}
// 88ms
var moveZeroes = function (nums) {
  const n = nums.length
  let j = 0
  for (let i = 0; i < n; i++) {
    // 前移非零数字，记录非零个数
    if (nums[i] !== 0) {
      const temp = nums[j]
      nums[j] = nums[i]
      nums[i] = temp
      j++
    }
  }
}
```

双指针版两数之和

> 这种方法不会漏掉的可能的值，应该是因为最大和最小的值分别在结尾和开头的两个数

```js
var twoSum = function (numbers, target) {
  let low = 0
  let high = numbers.length - 1
  while (low < high) {
    const sum = numbers[low] + numbers[high]
    if (sum === target) {
      return [low + 1, high + 1]
    }
    if (sum < target) {
      low++
    } else {
      high--
    }
  }
  return [-1, -1]
}
```

双指针：反转字符串

```js
var reverseString = function (s) {
  let i = 0
  let j = s.length - 1
  while (i < j) {
    // 1.用temp暂存
    const temp = s[i]
    s[i] = s[j]
    s[j] = temp
    // 2.数组结构复制
    // [s[i], s[j]] = [s[j], s[i]]
    i++
    j--
  }
}
```

反转字符串中的单词

```js
var reverseWords = function (s) {
  return s
    .split(' ')
    .map(i => i.split('').reverse().join(''))
    .join(' ')
}

// todo 。。。
var reverseWords = function (s) {
  const ret = []
  const length = s.length
  let i = 0
  while (i < length) {
    let start = i
    // 找到单词边界
    while (i < length && s[i] != ' ') {
      i++
    }
    // 反向填充找到的单词
    for (let p = start; p < i; p++) {
      ret.push(s[start + i - 1 - p])
    }
    // 判断边界，添加空格
    while (i < length && s[i] == ' ') {
      i++
      ret.push(' ')
    }
  }
  return ret.join('')
}
```

链表的中间结点 ？

双指针：无重复字符的最长子串

```js
// 460ms
var lengthOfLongestSubstring = function (s) {
  let n = 0
  let t = 0
  let arr = []
  let max = 0
  while (t < s.length) {
    if (!arr.includes(s[t])) {
      arr.push(s[t++])
    } else {
      n++
      t = n + 1
      max = Math.max(arr.length, max)
      arr = [s[n]]
    }
  }
  return Math.max(arr.length, max)
}
// 230ms 性能更好
var lengthOfLongestSubstring = function (s) {
  let n = 0
  let t = 0
  let arr = new Set()
  let max = 0
  while (t < s.length) {
    if (!arr.has(s[t])) {
      arr.add(s[t++])
    } else {
      n++
      t = n + 1
      max = Math.max(arr.size, max)
      arr.clear()
      arr.add(s[n])
    }
  }
  return Math.max(arr.size, max)
}
// 100ms
var lengthOfLongestSubstring = function (s) {
  // 哈希集合，记录每个字符是否出现过
  const occ = new Set()
  const n = s.length
  // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
  let rk = 0
  let ans = 0
  for (let i = 0; i < n; i++) {
    if (i != 0) {
      // 左指针向右移动一格，移除一个字符
      occ.delete(s[i - 1])
    }
    while (rk < n && !occ.has(s[rk])) {
      // 不断地移动右指针
      occ.add(s[rk])
      rk++
    }
    // 第 i 到 rk 个字符是一个极长的无重复字符子串
    ans = Math.max(ans, rk - i)
  }
  return ans
}
```
