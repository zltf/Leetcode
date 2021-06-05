# 1. 两数之和（简单）

## 暴力求解

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; ++i) {
            for (int j = i + 1; j < nums.length; ++j) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[0];
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.6 MB, 在所有 Java 提交中击败了59.78%的用户

## 哈希表

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] result = new int[2];
        HashMap<Integer,Integer> hashMap = new HashMap<>();
        for(int i = 0; i<nums.length; i++) {
            if(hashMap.containsKey(nums[i])) {
                result[0] = i;
                result[1] = hashMap.get(nums[i]);
                
                return result;
            }
            hashMap.put(target-nums[i], i);
        }
        return result;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.6 MB, 在所有 Java 提交中击败了56.65%的用户

# 3. 无重复字符的最长子串（中等）

## 滑动窗口

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int left = 0, right = 0;
        int max = 0;
        while(right < s.length()) {
            char ch = s.charAt(right);
            int pos = left;
            while(pos<right) {
                if(s.charAt(pos) == ch) {
                    break;
                }
                pos++;
            }
            if(pos != right) {
                left = pos+1;
            }
            right++;
            if(right-left > max) {
                max = right-left;
            }
        }
        return max;
    }
}
```

执行用时：2 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.8 MB, 在所有 Java 提交中击败了30.26%的用户

# 4. 寻找两个正序数组的中位数（困难）

## 半归并

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int targetId = nums1.length+nums2.length;
        boolean flag = targetId%2 == 0;             // 偶数项为true
        float result = 0;
        targetId /= 2;

        if(nums1.length == 0){
            return flag? (nums2[targetId]+nums2[targetId-1])/2f:nums2[targetId];
        }
        if(nums2.length == 0){
            return flag? (nums1[targetId]+nums1[targetId-1])/2f:nums1[targetId];
        }

        int ptr1 = 0, ptr2 = 0;
        while(targetId >= 0) {
            if(ptr2 == nums2.length) {
                if(flag && targetId == 0) {
                    result = (result+nums1[ptr1])/2;
                    return result;
                }
                result = nums1[ptr1];
                ++ptr1;
            } else if(ptr1 == nums1.length) {
                if(flag && targetId == 0) {
                    result = (result+nums2[ptr2])/2;
                    return result;
                }
                result = nums2[ptr2];
                ++ptr2;
            } else {
                if(nums1[ptr1] > nums2[ptr2]) {
                    if(flag && targetId == 0) {
                        result = (result+nums2[ptr2])/2;
                        return result;
                    }
                    result = nums2[ptr2];
                    ++ptr2;
                } else {
                    if(flag && targetId == 0) {
                        result = (result+nums1[ptr1])/2;
                        return result;
                    }
                    result = nums1[ptr1];
                    ++ptr1;
                }
            }
            --targetId;
        }
        return result;
    }
}
```

执行用时：2 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：39.6 MB, 在所有 Java 提交中击败了75.25%的用户

实际上是将复杂度不达标，要求O(log(m+n))

# 6. Z字形变换（中等）

## 按行访问

```java
class Solution {
    public String convert(String s, int numRows) {
        String res = "";
        // BaseCase
        // 1 <= numRows <= 1000（无0）
        // numRows-1 == 0 -> numRows == 1
        if(numRows == 1) {
            return s;
        }
        // 0*(numRows-1) + 2*(numRows-1) + 4*(numRows-1)......
        int i = 0;
        while(i*(numRows-1) < s.length()) {
            res += s.charAt(i*(numRows-1));
            i += 2;
        }
        // 0*(numRows-1)+1 + 2*(numRows-1)-1 + 2*(numRows-1)+1 + 4*(numRows-1)-1......
        // 0*(numRows-1)+2 + 2*(numRows-1)-2 + 2*(numRows-1)+2 + 4*(numRows-1)-2......
        // ......
        for(i = 1; i<numRows-1; i++) {
            int j = 0;
            boolean flag = false;
            int pos = j*(numRows-1)+i;
            while(pos < s.length()) {
                res += s.charAt(pos);
                if(flag) {
                    flag = false;
                    pos = j*(numRows-1)+i;
                } else {
                    j += 2;
                    flag = true;
                    pos = j*(numRows-1)-i;
                }
            }
        }
        // 1*(numRows-1) + 3*(numRows-1) + 5*(numRows-1)......
        i = 1;
        while(i*(numRows-1) < s.length()) {
            res += s.charAt(i*(numRows-1));
            i += 2;
        }
        return res;
    }
}
```

执行用时：17 ms, 在所有 Java 提交中击败了23.58%的用户

内存消耗：39.5 MB, 在所有 Java 提交中击败了13.94%的用户

# 11. 盛最多水的容器（中等）

## 左右双指针

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length-1;
        int max = 0;
        while(left < right) {
            int tmp = min(height[left], height[right]) * (right-left);
            if(tmp > max) {
                max = tmp;
            }
            if(height[left] > height[right]) {
                right--;
            } else {
                left++;
            }
        }
        return max;
    }

    private int min(int a, int b) {
        return a<b ? a : b;
    }
}
```

执行用时：4 ms, 在所有 Java 提交中击败了76.02%的用户

内存消耗：52.2 MB, 在所有 Java 提交中击败了5.08%的用户

# 26. 删除有序数组中的重复项（简单）

## 双指针法

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length == 0) {
            return 0;
        }
        int p1 = 0, p2 = 0;
        while (p2 != nums.length) {
            if (nums[p1] != nums[p2]) {
                nums[++p1] = nums[p2];
            }
            ++p2;
        }
        return ++p1;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了80.82%的用户

内存消耗：40.1 MB, 在所有 Java 提交中击败了87.19%的用户

# 37. 解数独（困难）

## 找每个位置可填入值，递归

```java
class Solution {
    boolean flag = false;

    public void solveSudoku(char[][] board) {
        for(int i = 0; i<9; i++) {
            for(int j = 0; j<9; j++) {
                if(board[i][j] == '.') {
                    boolean[] judgeRes = judge(board, i,j);
                    for(int k = 1; !flag && k<=9; k++) {
                        if(!judgeRes[k]) {
                            board[i][j] = (char)(k+'0');
                            solveSudoku(board);
                        }
                    }
                    if(!flag) {
                        board[i][j] = '.';
                        return;
                    }
                }
            }
        }

        flag = true;
        for(int i = 0; i<9; i++) {
            for(int j = 0; j<9; j++) {
                if(board[i][j] == '.') {
                    flag = false;
                }
            }
        }
    }

    public boolean[] judge(char[][] board, int x, int y) {
        // 为了计算方便，直接额外多一个空间
        boolean[] emnu = new boolean[10];
        for(int i = 0; i<9; i++) {
            if(board[x][i] != '.') emnu[board[x][i]-'0'] = true;
            if(board[i][y] != '.') emnu[board[i][y]-'0'] = true;
        }
        for(int i = x/3*3; i<x/3*3+3; i++) {
            for(int j = y/3*3; j<y/3*3+3; j++) {
                if(board[i][j] != '.') emnu[board[i][j]-'0'] = true;
            }
        }

        return emnu;
    }
}
```

执行用时：9 ms, 在所有 Java 提交中击败了38.88%的用户

内存消耗：38.2 MB, 在所有 Java 提交中击败了6.82%的用户

# 43. 字符串相乘（中等）

## 模拟竖式

```java
class Solution {
    public String multiply(String num1, String num2) {
        int c = 0;
        int tmp;
        String result = "";
        String strTmp = "";

        
        for(int i=0; i<num2.length(); i++) {
            // 计算一个数乘1位
            c = 0;
            strTmp = "";
            for(int j=num1.length()-1; j>=0; j--) {
                tmp = (num1.charAt(j)-'0')*(num2.charAt(i)-'0')+c;
                c = tmp/10;
                strTmp = tmp%10 + strTmp;
            }
            if(c != 0) {
                strTmp = c+strTmp;
            }

            result = plus((result+0), strTmp);
        }
        
        // 去除开头0
        c = 0;
        while(c != result.length()-1) {
            if(result.charAt(c) != '0') {
                break;
            }
            c++;
        }
        result = result.substring(c);
        return result;
    }

    public String plus(String num1, String num2) {
        int c = 0;
        int tmp;
        String result = "";
        
        int i = 0;
        while(i<num1.length() || i<num2.length()) {
            tmp = (i < num1.length() ? (num1.charAt(num1.length()-i-1)-'0') : 0)
                    + (i < num2.length() ? (num2.charAt(num2.length()-i-1)-'0') : 0)
                    + c;
            c = tmp/10;
            result = tmp%10 + result;
            i++;
        }
        if(c != 0) {
            result = c+result;
        }
        return result;
    }
}
```

执行用时：48 ms, 在所有 Java 提交中击败了7.96%的用户

内存消耗：39.4 MB, 在所有 Java 提交中击败了5.09%的用户

# 45. 跳跃游戏Ⅱ（中等）

## 动态规划

```java
class Solution {
    public int jump(int[] nums) {
        int[] dp = new int[nums.length];
        for(int i = 1; i<nums.length; i++) {
            dp[i] = Integer.MAX_VALUE;
        }
        dp[0] = 0;
        for(int i = 0; i<nums.length; i++) {
            for(int j = 1; j<=nums[i]; j++) {
                // 能到后面则必能到前面，到达最后一定是最小步数
                if(j+i>nums.length-1) {
                    return dp[nums.length-1];
                }
                dp[i+j] = min(dp[i]+1, dp[i+j]);
            }
        }
        return dp[nums.length-1];
    }

    public int min(int a, int b) {
        return a>b? b : a;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：36 MB, 在所有 Java 提交中击败了85.72%的用户

## 贪心算法

```java
class Solution {
    public int jump(int[] nums) {
        int last = nums.length-1;
        int result = 0;
        while(last != 0) {
            for(int i=0; i<last; i++) {
                if(i+nums[i] >= last) {
                    last = i;
                    result++;
                    break;
                }
            }
        }
        return result;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.8 MB, 在所有 Java 提交中击败了91.68%的用户

# 55. 跳跃游戏（中等）

## 找0，看能否跨0

```java
class Solution {
    public boolean canJump(int[] nums) {
        int zero = -1;
        for(int i = nums.length-1; i>=0; i--) {
            if(zero == -1 && nums[i] == 0) {
                zero = i;
            }
            if(zero != -1) {
                if(nums[i]+i > zero || nums[i]+i == nums.length-1) {
                    zero = -1;
                }
            }
        }
        return zero == -1;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了99.98%的用户

内存消耗：40.4 MB, 在所有 Java 提交中击败了51.07%的用户

# 61. 旋转链表（中等）

## 环形链表

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
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        // BaseCase
        if(head == null || head.next == null) {
            return head;
        }
        // 统计长度
        int len = 1;
        ListNode node;
        for(node = head; node.next != null; node = node.next) {
            len++;
        }
        // 环形链表
        node.next = head;
        // 寻找断点
        k = len-(k % len);
        for(int i = 0; i<k; i++) {
            node = node.next;
        }
        // 断开
        head = node.next;
        node.next = null;
        return head;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：37.5 MB, 在所有 Java 提交中击败了98.56%的用户

此题如用快慢指针还得算长度，耗时相同

# 70. 爬楼梯（简单）

## 动态规划

```java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i<=n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：34.9 MB, 在所有 Java 提交中击败了97.70%的用户

# 73. 矩阵置零（中等）

## 标记数组记在矩阵内

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        // 将第一个遇到的0点作为基准，得到基准坐标
        final int[] pos = findFirstZero(matrix);
        // 没有0
        if(pos == null) return;
        // 基准置0
        for(int i = 0; i<matrix.length; i++) {
            for(int j = 0; j<matrix[0].length; j++) {
                if(matrix[i][j] == 0) {
                    // 基准置0
                    matrix[i][pos[1]] = 0;
                    matrix[pos[0]][j] = 0;
                }
            }
        }
        // 纵向置横0
        for(int i = 0; i<matrix.length; i++) {
            if(i != pos[0] && matrix[i][pos[1]] == 0) {
                for(int j = 0; j<matrix[0].length; j++) {
                    matrix[i][j] = 0;
                }
            }
            matrix[i][pos[1]] = 0;
        }
        // 横向置纵0
        for(int i = 0; i<matrix[0].length; i++) {
            if(i != pos[1] && matrix[pos[0]][i] == 0) {
                for(int j = 0; j<matrix.length; j++) {
                    matrix[j][i] = 0;
                }
            }
            matrix[pos[0]][i] = 0;
        }
    }

    // 找首个0
    public int[] findFirstZero(int[][] matrix) {
        for(int i = 0; i<matrix.length; i++) {
            for(int j = 0; j<matrix[0].length; j++) {
                if(matrix[i][j] == 0) {
                    return new int[]{i, j};
                }
            }
        }
        return null;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了99.91%的用户

内存消耗：39.9 MB, 在所有 Java 提交中击败了74.10%的用户

# 74. 搜素二维矩阵（中等）

## 遍历行、列搜素

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        int i;
        for(i=0; i<m; i++) {
            if(matrix[i][0] > target) {
                break;
            }
        }
        if(i == 0) {
            return false;
        }
        for(int j=0; j<n; j++) {
            if(matrix[i-1][j] == target) {
                return true;
            } else if(matrix[i-1][j] > target) {
                return false;
            }
        }
        return false;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38 MB, 在所有 Java 提交中击败了46.40%的用户

# 80. 删除有序数组中的重复项（中等）

## 双指针

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length == 1) {
            return 1;
        }
        int fast = 2;
        int slow = 2;
        while(fast < nums.length) {
            if(nums[slow-2] != nums[fast]) {
                nums[slow++] = nums[fast];
            }
            fast++;
        }
        return slow;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.5 MB, 在所有 Java 提交中击败了75.75%的用户

# 81. 搜索旋转排序数组Ⅱ（中等）

## 二分法

```java
class Solution {
    public boolean search(int[] nums, int target) {
        if(nums.length == 0) {
            return false;
        }
        if(nums.length == 1) {
            return target == nums[0];
        }
        int start = 0;
        int end = nums.length-1;
        int mid;
        while(start <= end) {
            mid = start + (end - start) / 2;
            if(nums[mid] == target) {
                return true;
            }
            if(nums[start] == nums[mid] && nums[mid] == nums[end]) {
                start++;
                end--;
            } else if(nums[mid] > nums[end]) {  // 分界在右
                // 从左判断
                if(nums[mid] > target && target >= nums[start]) {
                    end = mid - 1;
                } else {
                    start = mid + 1;
                }
            } else {                            // 分界在左
                // 从右判断
                if(nums[end] >= target && target > nums[mid]) {
                    start = mid + 1;
                } else {
                    end = mid - 1;
                }
            }
        }
        return false;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了86.37%的用户

内存消耗：38.3 MB, 在所有 Java 提交中击败了54.57%的用户

# 82. 删除排序链表中的重复元素Ⅱ（中等）

## 面向测试用例编程，向前搜索2个节点

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
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null) return head;
        boolean flag = false;
        boolean headFlag = head.val == head.next.val;
        
        ListNode node = head;
        while(node.next != null && node.next.next != null) {
            while(node.next.next != null && node.next.next.val == node.next.val) {
                flag = true;
                node.next.next = node.next.next.next;
            }
            if(flag) {
                node.next = node.next.next;
                flag = false;
            } else {
                node = node.next;
            }
            if(node.next == null) {
                break;
            }
        }
        while(head.next != null && head.val == head.next.val) {
            head = head.next;
            flag = true;
        }
        if(headFlag) {
            head = head.next;
        }
        return head;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了64.24%的用户

内存消耗：38 MB, 在所有 Java 提交中击败了24.63%的用户

# 83. 删除排序链表中的重复元素（简单）

## 无需头节点迭代

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
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null) {
            return head;
        }
        ListNode node = head;
        while(node.next != null) {
            if(node.val == node.next.val) {
                node.next = node.next.next;
            } else {
                node = node.next;
            }
        }
        return head;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.1 MB, 在所有 Java 提交中击败了15.49%的用户

# 88. 合并两个有序数组（简单）

## 逆向单指针+循环

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int p1;
        for(int i=0; i<n; i++) {
            p1 = m-1;
            while(p1 >= 0 && nums1[p1] > nums2[i]) {
                nums1[p1+1] = nums1[p1];
                p1--;
            }
            m++;
            nums1[p1+1] = nums2[i];
        }
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了22.52%的用户

内存消耗：38.4 MB, 在所有 Java 提交中击败了79.75%的用户

## 极致压缩

```
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        while(n-1 >= 0) nums1[m+n-1] = m-1 >= 0 && nums1[m-1] > nums2[n-1] ? nums1[m---1] : nums2[n---1];
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.6 MB, 在所有 Java 提交中击败了35.94%的用户

# 90. 子集Ⅱ（中等）

## 排序循环追加

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<Integer>());
        for(Integer num : nums) {
            List<List<Integer>> tempRes = new ArrayList<>();
            for(List<Integer> list : res) {
                List<Integer> temp = new ArrayList<>();
                for(Integer i : list) {
                    temp.add(i);
                }
                temp.add(num);
                if(!inList(res, temp)) {
                    tempRes.add(temp);
                }
            }
            for(List<Integer> list : tempRes) {
                res.add(list);
            }
        }
        return res;
    }

    private boolean inList(List<List<Integer>> lists, List<Integer> target) {
        for(List<Integer> list : lists) {
            if(target.size() != list.size()) {
                continue;
            }
            boolean flag = true;
            for(int i=0; i<list.size(); i++) {
                if(list.get(i) != target.get(i)) {
                    flag = false;
                    break;
                }
            }
            if(flag) {
                return true;
            }
        }
        return false;
    }
}
```

执行用时：14 ms, 在所有 Java 提交中击败了5.41%的用户

内存消耗：38.8 MB, 在所有 Java 提交中击败了48.87%的用户

# 94. 二叉树的中序遍历（中等）

## 递归法

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorder(result, root);
        return result;
    }

    public void inorder(List<Integer> list, TreeNode root) {
        if(root == null) {
            return;
        }
        inorder(list, root.left);
        list.add(root.val);
        inorder(list, root.right);
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：36.7 MB, 在所有 Java 提交中击败了53.68%的用户

## 栈迭代

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();

        while(root != null || !stack.isEmpty()) {
            while(root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            res.add(root.val);
            root = root.right;
        }

        return res;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了42.32%的用户

内存消耗：36.7 MB, 在所有 Java 提交中击败了55.95%的用户

# 153. 寻找旋转排序数组中的最小值（中等）

## 遍历找拐点

```java
class Solution {
    public int findMin(int[] nums) {
        if(nums[nums.length-1] >= nums[0]) {
            return nums[0];
        }
        for(int i=1; i<nums.length; i++) {
            if(nums[i-1] > nums[i]) {
                return nums[i];
            }
        }
        return 0;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：37.9 MB, 在所有 Java 提交中击败了55.94%的用户

## 二分查找法

```java
class Solution {
    public int findMin(int[] nums) {
        if(nums[nums.length-1] >= nums[0]) {
            return nums[0];
        }
        int start = 1, end = nums.length-1;
        int mid = (start + end) / 2;
        while(start <= end) {
            if(nums[mid-1] > nums[mid]) {
                return nums[mid];
            }
            if(nums[mid] > nums[end]) {
                start = mid+1;
                mid = (start + end) / 2;
            } else {
                end = mid-1;
                mid = (start + end) / 2;
            }
        }
        return 0;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：37.8 MB, 在所有 Java 提交中击败了70.09%的用户

# 154. 寻找旋转排序数组中的最小值Ⅱ（困难）

## 遍历找拐点（上一题的改版）

```java
class Solution {
    public int findMin(int[] nums) {
        if(nums.length == 1) {
            return nums[0];
        }
        if(nums[nums.length-1] > nums[0]) {
            return nums[0];
        }
        for(int i=1; i<nums.length; i++) {
            if(nums[i-1] > nums[i]) {
                return nums[i];
            }
        }
        return nums[0];
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.5 MB, 在所有 Java 提交中击败了10.62%的用户

# 160. 相交链表（简单）

## 双栈法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Stack<ListNode> stackA= new Stack<>();
        Stack<ListNode> stackB= new Stack<>();
        for (ListNode node=headA; node!=null; node=node.next) {
            stackA.push(node);
        }
        for (ListNode node=headB; node!=null; node=node.next) {
            stackB.push(node);
        }
        ListNode res = null;
        while (!stackA.empty() && !stackB.empty() && stackA.peek() == stackB.peek()) {
            res = stackA.peek();
            stackA.pop();
            stackB.pop();
        }
        return res;
    }
}
```

执行用时：3 ms, 在所有 Java 提交中击败了20.23%的用户

内存消耗：41.3 MB, 在所有 Java 提交中击败了50.32%的用户

# 173. 二叉搜索树迭代器（中等）

## 迭代法中序

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
class BSTIterator {

    Stack<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        stack = new Stack<>();
        while(root != null) {
            stack.push(root);
            root = root.left;
        }
    }
    
    // 总是有效的
    public int next() {
        TreeNode node = stack.pop();
        int res = node.val;
        if(node.right != null) {
            node = node.right;
            while(node != null) {
                stack.push(node);
                node = node.left;
            }
        }
        return res;
    }
    
    public boolean hasNext() {
        return !stack.isEmpty();
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

执行用时：27 ms, 在所有 Java 提交中击败了15.51%的用户

内存消耗：42.3 MB, 在所有 Java 提交中击败了7.36%的用户

# 179. 最大数（中等）

## 快排

```java
class Solution {
    public String largestNumber(int[] nums) {
        quickSort(nums, 0, nums.length-1);
        StringBuilder res = new StringBuilder();
        if(nums[0] == 0) {
            return "0";
        }
        for(int num : nums) {
            res.append(num);
        }
        return res.toString();
    }

    private void quickSort(int[] nums, int start, int end) {
        if(end - start <= 0) {
            return;
        }
        int left = start;
        int right = end;
        int tmp = nums[start];
        while(left < right) {
            while(left < right && !compare(nums[right], tmp)) {
                right--;
            }
            nums[left] = nums[right];
            while(left < right && compare(nums[left], tmp)) {
                left++;
            }
            nums[right] = nums[left];
        }
        nums[left] = tmp;
        quickSort(nums, start, left-1);
        quickSort(nums, right+1, end);
    }

    private boolean compare(int a, int b) {
        String strA = String.valueOf(a);
        String strB = String.valueOf(b);
        return Long.parseLong(strA + strB) > Long.parseLong(strB + strA);
    }
}
```

执行用时：9 ms, 在所有 Java 提交中击败了27.05%的用户

内存消耗：38.6 MB, 在所有 Java 提交中击败了7.03%的用户

# 190. 颠倒二进制位（简单）

## 移位运算

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int res = 0;
        for(int i=0; i<32; i++) {
            res <<= 1;
            res += n & 1;
            n >>>= 1;
        }
        return res;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：37.6 MB, 在所有 Java 提交中击败了99.94%的用户

## 位运算分治

```java
public class Solution {
    // you need treat n as an unsigned value

    private final int M1 = 0x55555555;
    private final int M2 = 0x33333333;
    private final int M3 = 0x0f0f0f0f;
    private final int M4 = 0x00ff00ff;

    public int reverseBits(int n) {
        n = (n & ~M1) >>> 1 | (n & M1) << 1;
        n = (n & ~M2) >>> 2 | (n & M2) << 2;
        n = (n & ~M3) >>> 4 | (n & M3) << 4;
        n = (n & ~M4) >>> 8 | (n & M4) << 8;
        return n >>> 16 | n << 16;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.1 MB, 在所有 Java 提交中击败了77.16%的用户

# 191. 位1的个数（简单）

## 右移

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int res = 0;
        while(n!=0) {
            if((n>>1)<<1 != n) {
                res++;
            }
            n >>>= 1;
        }
        return res;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了95.76%的用户

内存消耗：35.4 MB, 在所有 Java 提交中击败了34.17%的用户

## 位运算优化

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int res = 0;
        while(n!=0) {
            // XXXX1000 & XXXX0111 = XXXX0000
            // 最低位1置0
            n &= n-1;
            res++;
        }
        return res;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了95.76%的用户

内存消耗：35.2 MB, 在所有 Java 提交中击败了85.39%的用户

# 200. 岛屿数量（中等）

## 感染法

```java
class Solution {
    public int numIslands(char[][] grid) {
        int res = 0;

        for(int i = 0; i<grid.length; i++) {
            for(int j = 0; j<grid[0].length; j++) {
                if(grid[i][j] == '1') {
                    res++;
                    grid[i][j] = '1';
                    spread(grid, i, j);
                }
            }
        }

        return res;
    }

    public void spread(char[][] grid, int x, int y) {
        if(x>0) {
            if(grid[x-1][y] == '1') {
                grid[x-1][y] = '2';
                spread(grid, x-1, y);
            }
        }
        if(x<grid.length-1) {
            if(grid[x+1][y] == '1') {
                grid[x+1][y] = '2';
                spread(grid, x+1, y);
            }
        }
        if(y>0) {
            if(grid[x][y-1] == '1') {
                grid[x][y-1] = '2';
                spread(grid, x, y-1);
            }
        }
        if(y<grid[0].length-1) {
            if(grid[x][y+1] == '1') {
                grid[x][y+1] = '2';
                spread(grid, x, y+1);
            }
        }
    }
}
```

执行用时：2 ms, 在所有 Java 提交中击败了92.84%的用户

内存消耗：40.7 MB, 在所有 Java 提交中击败了80.55%的用户

# 208. 实现Trie（前缀树）（中等）

## 孩子链表多叉树

```java
class Trie {
    TrieNode root;

    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode node = root;
        for(int i=0; i<word.length(); i++) {
            TrieNode searchNode = node.searchChild(word.charAt(i));
            if(searchNode == null) {
                searchNode = new TrieNode(word.charAt(i));
                node.addChild(searchNode);
            }
            node = searchNode;
        }
        node.fin = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode node = root;
        for(int i=0; i<word.length(); i++) {
            TrieNode searchNode = node.searchChild(word.charAt(i));
            if(searchNode == null) {
                return false;
            }
            node = searchNode;
        }
        if(node.fin) {
            return true;
        }
        return false;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode node = root;
        for(int i=0; i<prefix.length(); i++) {
            TrieNode searchNode = node.searchChild(prefix.charAt(i));
            if(searchNode == null) {
                return false;
            }
            node = searchNode;
        }
        return true;
    }
}

class TrieNode {
    char ch;
    List<TrieNode> nodes;
    boolean fin;

    public TrieNode() {
        nodes = new ArrayList<>();
        fin = false;
    }

    public TrieNode(char ch) {
        this.ch = ch;
        nodes = new ArrayList<>();
        fin = false;
    }
    
    public void addChild(TrieNode child) {
        nodes.add(child);
    }

    public TrieNode searchChild(char ch) {
        for(TrieNode node : nodes) {
            if(node.ch == ch) {
                return node;
            }
        }
        return null;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

执行用时：56 ms, 在所有 Java 提交中击败了21.77%的用户

内存消耗：47.4 MB, 在所有 Java 提交中击败了84.87%的用户

# 213. 打家劫舍Ⅱ（中等）

## 去掉头尾分别动态规划

```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 0) {
            return 0;
        } else if(nums.length == 1) {
            return nums[0];
        }

        int[] dp = new int[nums.length];
        
        dp[0] = 0;
        dp[1] = nums[1];
        for(int i=2; i<nums.length; i++) {
            dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
        }

        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for(int i=2; i<nums.length-1; i++) {
            dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
        }
        
        return max(dp[nums.length-1], dp[nums.length-2]);
    }

    private int max(int a, int b) {
        return a>b ? a : b;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.7 MB, 在所有 Java 提交中击败了85.43%的用户

# 263. 丑数（简单）

```java
class Solution {
    public boolean isUgly(int n) {
        if(n == 0) {
            return false;
        }
        while(n % 2 == 0) {
            n /= 2;
        }
        while(n % 3 == 0) {
            n /= 3;
        }
        while(n % 5 == 0) {
            n /= 5;
        }
        return n == 1;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.4 MB, 在所有 Java 提交中击败了75.85%的用户

# 264. 丑数Ⅱ（中等）

## 三指针（动态规划：arr当dp）

```java
class Solution {
    public int nthUglyNumber(int n) {
        List<Integer> arr = new ArrayList<>();
        arr.add(1);
        int id2 = 0;
        int id3 = 0;
        int id5 = 0;
        while(n>1) {
            int tmp = min(arr.get(id2) * 2, arr.get(id3) * 3, arr.get(id5) * 5);
            if(tmp == arr.get(id2) * 2) {
                id2++;
            }
            if(tmp == arr.get(id3) * 3) {
                id3++;
            }
            if(tmp == arr.get(id5) * 5) {
                id5++;
            }
            arr.add(tmp);
            n--;
        }
        return arr.get(arr.size()-1);
    }

    private int min(int a, int b, int c) {
        int tmp = a<b ? a : b;
        return tmp<c ? tmp : c;
    }
}
```

执行用时：10 ms, 在所有 Java 提交中击败了31.63%的用户

内存消耗：38.2 MB, 在所有 Java 提交中击败了9.72%的用户

## 小根堆

```java
class Solution {
    public int nthUglyNumber(int n) {
        List<Long> heap = new ArrayList<>();
        long num = 0;
        heap.add(1L);
        while(n > 1) {
            num = heap.get(0);
            while(heap.size()>0 && num == heap.get(0)) {
                popHeap(heap);
            }
            pushHeap(heap, num * 2);
            pushHeap(heap, num * 3);
            pushHeap(heap, num * 5);
            n--;
        }
        return (int)(long)heap.get(0);
    }

    private void pushHeap(List<Long> heap, long x) {
        heap.add(x);
        int id = heap.size()-1;
        while(id != 0) {
            int i1 = (id-1) / 2;
            if(heap.get(i1) > heap.get(id)) {
                swap(heap, i1, id);
                id = i1;
            } else {
                break;
            }
        }
    }

    private void popHeap(List<Long> heap) {
        heap.set(0, heap.get(heap.size()-1));
        heap.remove(heap.size()-1);
        int id = 0;
        while(id*2+1 < heap.size()) {
            int minId = id*2+1;
            if(minId+1 < heap.size() && heap.get(minId+1) < heap.get(minId)) {
                minId++;
            }
            if(heap.get(minId) < heap.get(id)) {
                swap(heap, minId, id);
                id = minId;
            } else {
                break;
            }
        }
    }

    private void swap(List<Long> heap, int i1, int i2) {
        long tmp = heap.get(i1);
        heap.set(i1, heap.get(i2));
        heap.set(i2, tmp);
    }
}
```

执行用时：215 ms, 在所有 Java 提交中击败了7.82%的用户

内存消耗：38.2 MB, 在所有 Java 提交中击败了12.14%的用户

# 342. 4的幂（简单）

## 除4迭代

```java
class Solution {
    public boolean isPowerOfFour(int n) {
        if (n < 1) {
            return false;
        }
        while (n != 1) {
            if (n % 4 != 0) {
                return false;
            } else {
                n = n / 4;
            }
        }
        return true;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.6 MB, 在所有 Java 提交中击败了41.90%的用户

## 位运算

```java
class Solution {
    public boolean isPowerOfFour(int n) {
        if(n < 1) return false;
        while (n != 1) {
            if ((n & 3) != 0) {
                return false;
            }
            n >>= 2;
        }
        return true;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.3 MB, 在所有 Java 提交中击败了80.90%的用户

## 位运算掩码

```java
class Solution {
    public boolean isPowerOfFour(int n) {
        return n > 0 && (n & (n-1)) == 0 && (n & 0xAAAAAAAA) == 0;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.5 MB, 在所有 Java 提交中击败了52.61%的用户

# 456. 132模式（中等）

## 单调栈

```java
class Solution {
    public boolean find132pattern(int[] nums) {
        int k = 0;
        Stack<Integer> stack = new Stack<>();
        for(int i = nums.length-1; i>=0; i--) {
            if(k!=0 && nums[k] > nums[i]) {
                return true;
            }
            while(!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
                k = stack.pop();
            }
            stack.push(i);
        }
        return false;
    }
}
```

执行用时：9 ms, 在所有 Java 提交中击败了43.28%的用户

内存消耗：39.3 MB, 在所有 Java 提交中击败了7.47%的用户

# 525. 连续数组（中等）

## 前缀和+哈希表

```java
class Solution {
    public int findMaxLength(int[] nums) {
        int tmp = 0;
        int maxDis = 0;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);
        for (int i=0; i<nums.length; i++) {
            if (nums[i] == 1) {
                tmp++;
            } else {
                tmp--;
            }
            if (map.containsKey(tmp)) {
                maxDis = i - map.get(tmp) > maxDis ? i - map.get(tmp) : maxDis;
            } else {
                map.put(tmp, i);
            }
        }
        return maxDis;
    }
}
```

执行用时：28 ms, 在所有 Java 提交中击败了46.79%的用户

内存消耗：48.2 MB, 在所有 Java 提交中击败了33.70%的用户

# 532. 连续的子数组和（中等）

## 暴力法遍历所有子数组

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        for (int i = 0; i < nums.length; i++) {
            int sum = nums[i];
            for(int j = i + 1; j < nums.length; j++) {
                sum += nums[j];
                if (sum % k == 0) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

超出时间限制

## 前缀和+哈希表

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);
        int remainder = 0;
        for (int i=0; i<nums.length; i++) {
            remainder = (remainder + nums[i]) % k;
            if (map.containsKey(remainder)) {
                if (i - map.get(remainder) >= 2) {
                    return true;
                }
            } else {
                map.put(remainder, i);
            }
        }
        return false;
    }
}
```

执行用时：20 ms, 在所有 Java 提交中击败了54.79%的用户

内存消耗：54.8 MB, 在所有 Java 提交中击败了9.24%的用户

# 650. 只有两个键的键盘（中等）

## 递归

```java
class Solution {
    public int minSteps(int n) {
        if(n == 1) {
            return 0;
        }
        int i = 2;
        while(n%i != 0) {
            i++;
        }
        // c + v*(i-1) = i*操作
        return i + minSteps(n/i);
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：34.9 MB, 在所有 Java 提交中击败了98.42%的用户

## 分解素因数

```java
class Solution {
    public int minSteps(int n) {
        int res = 0;
        int i = 2;
        while(n != 1) {
            while(n%i == 0) {
                res += i;
                n /= i;
            }
            i++;
        }
        return res;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：34.9 MB, 在所有 Java 提交中击败了99.70%的用户

# 781. 森林中的兔子（中等）

## 计数

```java
class Solution {
    public int numRabbits(int[] answers) {
        int count = 0;
        int num = -1;
        int res = 0;
        Arrays.sort(answers);
        for(int n : answers) {
            if(num != n) {
                res += num+1;
                num = n;
                count = 1;
            } else if(count == num+1) {
                res += count;
                count = 1;
            } else {
                count++;
            }
        }
        res += num+1;
        return res;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了96.97%的用户

内存消耗：37.9 MB, 在所有 Java 提交中击败了48.10%的用户

# 783. 二叉搜索树节点最小距离（简单）

## 中序遍历

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
    public int minDiffInBST(TreeNode root) {
        int last = -1;
        int minDist = Integer.MAX_VALUE;
        Stack<TreeNode> stack = new Stack<>();
        while(root != null || !stack.isEmpty()) {
            while(root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if(last != -1 && root.val-last < minDist) {
                minDist = root.val-last;
            }
            last = root.val;
            root = root.right;
        }
        return minDist;
    }
}
```

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：35.9 MB, 在所有 Java 提交中击败了74.28%的用户

# 1006. 笨阶乘（中等）

## 分组计算

```java
class Solution {
    public int clumsy(int N) {
        int res = 0;
        int i = N;
        while(i>0) {
            int tmp = i;
            i--;
            if(i>0) {
                tmp *= i;
            }
            i--;
            if(i>0) {
                tmp /= i;
            }
            i--;
            if(i == N-3) {
                res += tmp;
            } else {
                res -= tmp;
            }
            if(i>0) {
                res += i;
            }
            i--;
        }
        return res;
    }
}
```

执行用时：2 ms, 在所有 Java 提交中击败了47.10%的用户

内存消耗：35.4 MB, 在所有 Java 提交中击败了37.42%的用户

# 面试题17.21. 直方图的水量（困难）

## 双向搜索

```java
class Solution {
    public int trap(int[] height) {
        int res = 0;
        // 从左往右
        for(int i=1; i<height.length; i++) {
            if(height[i] < height[i-1]) {
                for(int j=i+1; j<height.length; j++) {
                    if(height[j] >= height[i-1]) {
                        for(int k=i; k<j; k++) {
                            res += height[i-1] - height[k];
                            height[k] = height[i-1];
                        }
                        i = j;
                        break;
                    }
                }
            }
        }
        // 从右往左
        for(int i=height.length-2; i>=0; i--) {
            if(height[i] < height[i+1]) {
                for(int j=i-1; j>=0; j--) {
                    if(height[j] >= height[i+1]) {
                        for(int k=i; k>j; k--) {
                            res += height[i+1] - height[k];
                        }
                        i = j;
                        break;
                    }
                }
            }
        }
        return res;
    }
}
```

执行用时：1 ms, 在所有 Java 提交中击败了99.90%的用户

内存消耗：37.8 MB, 在所有 Java 提交中击败了92.19%的用户