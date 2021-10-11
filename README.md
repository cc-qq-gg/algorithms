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
// 100ms 滑动窗口
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

双指针，滑动窗口：字符串的排列

```js
// 性能极差 7505ms
var checkInclusion = function (s1, s2) {
  const target = Array.from(s1).sort().toString()
  let n2 = s2.length
  let n1 = s1.length
  let r = n1
  let temp = Array.from(s2.slice(0, n1))
  while (r <= n2) {
    if (temp.slice().sort().toString() === target) {
      return true
    }
    temp.shift()
    temp.push(s2[r++])
  }
  return false
}
// 160ms，统计字母个数
var checkInclusion = function (s1, s2) {
  const n = s1.length,
    m = s2.length
  if (n > m) {
    return false
  }
  const cnt1 = new Array(26).fill(0)
  const cnt2 = new Array(26).fill(0)
  const aCharCode = 'a'.charCodeAt()
  for (let i = 0; i < n; ++i) {
    ++cnt1[s1[i].charCodeAt() - aCharCode]
    ++cnt2[s2[i].charCodeAt() - aCharCode]
  }
  if (cnt1.toString() === cnt2.toString()) {
    return true
  }
  // 滑动窗口，每次比较
  for (let i = n; i < m; ++i) {
    ++cnt2[s2[i].charCodeAt() - aCharCode]
    --cnt2[s2[i - n].charCodeAt() - aCharCode]
    if (cnt1.toString() === cnt2.toString()) {
      return true
    }
  }
  return false
}

// 80ms 性能好到令人胆寒
var checkInclusion = function checkInclusion(s1, s2) {
  const n = s1.length
  const m = s2.length
  const aCharCode = 'a'.charCodeAt()
  const getIndex = s => s.charCodeAt() - aCharCode
  if (n > m) {
    return false
  }
  // 用一个长度26的数组记录每个字母的差异，都为零则存在相等排列
  const cnt = new Array(26).fill(0)
  // 记录不同位的个数，首次挖坑填坑
  let diff = 0
  for (let i = 0; i < n; i++) {
    // 挖
    --cnt[getIndex(s1[i])]
    // 补
    ++cnt[getIndex(s2[i])]
  }
  for (const n of cnt) {
    if (n !== 0) {
      diff++
    }
  }
  if (diff === 0) {
    return true
  }
  // 移动窗口，尝试补坑
  for (let i = n; i < m; i++) {
    // 出字母位置
    const x = getIndex(s2[i - n])
    // 进字母位置
    const y = getIndex(s2[i])
    // 如果进出相同，跳过
    if (x === y) {
      continue
    }
    // 之前相等
    if (cnt[x] === 0) {
      diff++
    }
    // 移出窗口
    --cnt[x]
    // 之后相等
    if (cnt[x] === 0) {
      diff--
    }

    // y同理
    if (cnt[y] === 0) {
      diff++
    }
    // 进入窗口
    ++cnt[y]
    // 之后相等
    if (cnt[y] === 0) {
      diff--
    }

    // 如果坑被填完
    if (diff === 0) {
      return true
    }
  }
  return false
}
```

深度优先，广度优先：岛屿最大面积

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int ans = 0;
        // 嵌套循环，遍历每项
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j != grid[0].length; ++j) {
                ans = Math.max(ans, dfs(grid, i, j));
            }
        }
        return ans;
    }

    public int dfs(int[][] grid, int cur_i, int cur_j) {
        // 判断边界，是否土地
        if (cur_i < 0 || cur_j < 0 || cur_i == grid.length || cur_j == grid[0].length || grid[cur_i][cur_j] != 1) {
            return 0;
        }
        grid[cur_i][cur_j] = 0;
        int[] di = {0, 0, 1, -1};
        int[] dj = {1, -1, 0, 0};
        int ans = 1;
        // 递归四个方向
        for (int index = 0; index < 4; ++index) {
            int next_i = cur_i + di[index];
            int next_j = cur_j + dj[index];
            ans += dfs(grid, next_i, next_j);
        }
        return ans;
    }
}
```

```js
var maxAreaOfIsland = function (grid) {
  let ans = 0
  const row = [1, -1, 0, 0]
  const col = [0, 0, -1, 1]
  const dfs = (grid, i, j) => {
    // 判断边界以及是否为土地
    if (i < 0 || i === grid.length || j < 0 || j === grid[0].length || grid[i][j] !== 1) {
      return 0
    }
    // 保证每个点只查一次
    grid[i][j] = 0
    // 记录每个点的相邻土地节点
    let ans = 1
    // 递归查找四个方向
    for (let n = 0; n < 4; n++) {
      ans += dfs(grid, i + row[n], j + col[n])
    }
    return ans
  }
  grid.forEach((_, i) => {
    grid[i].forEach((_, j) => {
      ans = Math.max(ans, dfs(grid, i, j))
    })
  })
  return ans
}
```

深度优先：图像渲染

```js
// 80ms
var floodFill = function (image, sr, sc, newColor) {
  const current = image[sr][sc]
  const row = [1, -1, 0, 0]
  const col = [0, 0, -1, 1]
  const rn = image.length
  const cn = image[0].length
  const dfs = (image, r, c) => {
    if (r >= 0 && c >= 0 && r < rn && c < cn) {
      if (image[r][c] === current) {
        image[r][c] = newColor
        for (let i = 0; i < 4; i++) {
          dfs(image, r + row[i], c + col[i])
        }
      }
    }
  }
  if (current !== newColor) {
    // 从中心点开始查询
    dfs(image, sr, sc)
  }
  return image
}
```

```java
// 1ms
class Solution {

    int[] row = {1, -1, 0, 0};
    int[] col = {0, 0, 1, -1};

    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        int color = image[sr][sc];
        if (color != newColor) {
            dfs(image, sr, sc, color, newColor);
        }
        return image;
    }

    public void dfs(int[][] image, int sr, int sc, int color, int newColor) {
        if (sr >= 0 && sc >= 0 && sr < image.length && sc < image[0].length) {
            if (image[sr][sc] == color) {
                image[sr][sc] = newColor;
                for (int i = 0; i < 4; i++) {
                    dfs(image, sr + row[i], sc + col[i], color, newColor);
                }
            }
        }
    }
}
```

深度优先：合并二叉树

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
      if(root1 == null) {
          return root2;
      }
      if(root2 == null) {
          return root1;
      }
      TreeNode merged = new TreeNode(root1.val + root2.val);
      merged.left = mergeTrees(root1.left, root2.left);
      merged.right = mergeTrees(root1.right, root2.right);
      return merged;
    }
}
```

广度优先：腐烂的橘子

```js
var orangesRotting = function (grid) {
  // 队列，广度优先搜索
  const rn = grid.length
  const cn = grid[0].length
  const row = [1, -1, 0, 0]
  const col = [0, 0, 1, -1]
  // 遍历队列
  const qune = []
  // 轮数
  let round = 0
  // 新鲜数
  let fresh = 0
  for (let r = 0; r < rn; r++) {
    for (let c = 0; c < cn; c++) {
      const current = grid[r][c]
      if (current === 1) {
        fresh++
      } else if (current === 2) {
        // 第一轮
        qune.push([r, c])
      }
    }
  }
  // 存在新鲜且队列不为空时继续遍历
  while (fresh > 0 && qune.length) {
    round++
    const n = qune.length
    for (let i = 0; i < n; i++) {
      // 当push和pop或 unshift 和 shift的时候，结果错误
      // 上述原因是，后进后出的方式是错的，导致这一轮用到下一轮的数据，
      const [r, c] = qune.shift()
      for (let j = 0; j < 4; j++) {
        const _r = r + row[j]
        const _c = c + col[j]
        if (_r >= 0 && _c >= 0 && _r < rn && _c < cn) {
          if (grid[_r][_c] === 1) {
            fresh--
            grid[_r][_c] = 2
            qune.push([_r, _c])
          }
        }
      }
    }
  }
  if (fresh !== 0) {
    return -1
  }
  return round
}
```

递归：合并两个有序列表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
//  0ms ? 递归， 时间空间复杂杜都是O(m + n)
// 判断 l1 和 l2 哪一个链表的头节点的值更小，然后递归地决定下一个添加到结果里的节点
// 为空的时候退出递归
// not clearly
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        if(l1.val < l2.val) {
          l1.next = mergeTwoLists(l1.next, l2);
          return l1;
        } else {
          l2.next = mergeTwoLists(l2.next, l1);
          return l2;
        }
    }
}
// 迭代的方式比较好东懂些
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);
        // 指针
        ListNode pre = prehead;
        while(l1 != null && l2 != null){
           if(l1.val <= l2.val) {
               pre.next = l1;
               // 移动指针
               l1 = l1.next;
           } else {
               pre.next = l2;
               // 移动指针
               l2 = l2.next;
           }
           // 移动指针
           pre = pre.next;
        }

        // 相当长度迭代完成后剩下的部分
        pre.next = l1 == null ? l2 : l1;

        return prehead.next;
    }
}
```
