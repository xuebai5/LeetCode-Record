# LeetCode-Record
LeetCode笔记

## <a name="Catalogue"/>目录
* Easy
 * [292.Nim Game](#292.Nim Game)
 * [258.Add Digits](#258.Add Digits)
 * [104.Maximum Depth of Binary Tree](#104.Maximum Depth of Binary Tree)


## <a name="292.Nim Game"/>292.Nim Game
###问题：

> You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

>Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

>For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

###大意：

> 你和你的一个朋友玩一个游戏：在一个桌子上有一堆石头，每个人每次可以从中拿1个或2个或3个石头，你和你朋友两个人轮流拿，谁拿走的桌上最后一波石头，谁就赢了。你先拿。
> 假设你和你的朋友都会采取最佳策略。写一个函数来判断对于给出的一个数量的石头，你会不会赢。
> 比如说，如果桌上有4个石头，那么你肯定输，无论你先拿1个还是2个还是3个，最后一波石头总是被你朋友拿走的。

###思路：
乍一看好像不递归不可能判断出来，但其实演算一下，发现是有规律可行的。

1. 首先当石头数量是3个以下时，你肯定会赢。
2. 当石头数量为4个时，你肯定输。
3. 当石头数量为5、6、7个时，你总是可以拿到桌上只剩4个，那对于你朋友来说，他面对4个石头，上面说了，一定会输，所以你一定会赢。
4. 当石头数量是8个时，你不管怎么拿，最终桌上只会剩下5、6、7三种情况，那么此时你朋友面对的就是情况3，他就一定赢，而你一定输。
5. 当石头数量是9、10、11时，你总是可以把石头拿到只剩8个，那么此时你朋友面对的就是情况4，那他就一定输，而你一定赢。
……

这样分析一下，规律就出来了，只要你面对的石头数量是4的倍数，那你就一定会输，此外，你全都可以赢，赢面还是很大啊哈哈。代码也呼之欲出了。

###代码（C++）：

```C++
class Solution {
public:
    bool canWinNim(int n) {
        return n % 4 == 0 ? false : true;
    }
};
```

示例工程：[https://github.com/Cloudox/NimGame](https://github.com/Cloudox/NimGame)

[回到目录](#Catalogue)

-------------------------

## <a name="258.Add Digits"/>258.Add Digits
###问题：

> Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

>For example:

>Given num = 38, the process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.

>Follow up:
>Could you do it without any loop/recursion in O(1) runtime?

###大意：
>给一个非负数，重复地加其中的数字知道最后只有一个数字。
>比如说：
>给出数字38，过程类似于：3+8=11,1+1=2.直到2只有一个数字了，就返回它。
>继续：
>你能不能不用任何循环、递归来在O(1)的时间复杂度中完成它？

###思路一：
首先想到的就是循环，对于一个数字，循环将其除以10的余数加起来，直到其是个位数。加完一次后判断是不是只有一个数字，也就是是不是小于10，如果还大于，说明还有多个数字，那就再进行同样的操作。这个方法我自己测试倒是对的，不过leetcode总是说我超时，而且是在11这个数时就超时。。

###代码一（c++）：

```c++
class Solution {
public:
    int addDigits(int num) {
        int tempResult = num;
		while (tempResult > 10) {
		    int tempNum = tempResult;
		    tempResult = 0;
       
	        for (; tempNum / 10 >= 1;) {
			    tempResult += tempNum % 10;
			    tempNum = tempNum / 10;
		    }
		    tempResult += tempNum;
	    }
		return tempResult;
    }
};
```

###思路二：
既然题目里也鼓励我们继续思考不用循环的方式，那就一定是有规律可循。我们可以简单地列一下：

数字 | 结果
----|------
0 | 0
1 | 1
2 | 2
3 | 3
4 | 4
5 | 5
6 | 6
7 | 7
8 | 8
9 | 9
10 | 1
11 | 2
12 | 3
13 | 4
14 | 5
15 | 6
16 | 7
17 | 8
18 | 9
19 | 1
20 | 2
21 | 3

就列这么多了，我们可以发现，结果除了第一个0以外，都在1~9之间循环。而且可以发现，其除以9的余数，为0时，对应的结果是9，而不为0时，余数等于对应的结果，那么代码就呼之欲出啦~

###代码二（c++）：

```c++
class Solution {
public:
    int addDigits(int num) {
        return num % 9 == 0 ? (num == 0 ? 0 : 9) : num % 9;
    }
};
```

示例工程：[https://github.com/Cloudox/AddDigits](https://github.com/Cloudox/AddDigits)

[回到目录](#Catalogue)

-------------------------

## <a name="104.Maximum Depth of Binary Tree"/>104.Maximum Depth of Binary Tree
###问题：
>Given a binary tree, find its maximum depth.

>The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

###大意：
>给出一个二叉树，找到其最大的深度。
>最大深度是指从根节点到最远的叶子节点的最长距离的节点数。

###思路：
要探索二叉树的深度，用递归比较方便。我们题目要求的函数返回根节点的深度，那么就做到对二叉树上每个节点调用此函数都返回其作为根节点看待时的深度。比如，所有叶子节点的深度都是1，再往上就是2、3...一直到root根节点的返回值就是最大的深度。
对于每个节点，我们先判断其本身是否是节点，如果是一个空二叉树，那么就应该返回0。
然后，我们定义两个变量，一个左节点深度，一个右节点深度。我们分别判断其有无左节点和右节点，两种节点中的做法都是一样的，假设没有左节点，那么就左节点深度变量就是1，有左节点的话，左节点深度变量就是对左节点调用此函数返回的结果加1；对右节点也做同样的操作。
最后比较左节点深度和右节点深度，判断谁比较大，就返回哪个变量。这样就能一层一层地递归获取最大深度了。

###代码（Java）：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int maxDepth(TreeNode root) {
        if (root != null) {// 有此节点
	        int rightResult;
	        int leftResult;
	        if (root.left != null) {// 有左节点
				leftResult = maxDepth(root.left) + 1;
			} else {// 无左节点
				leftResult = 1;
			}
			if (root.right != null) {// 有右节点
				rightResult = maxDepth(root.right) + 1;
			} else {// 无右节点
				rightResult = 1;
			}
			// 判断哪边更深，返回更深的深度
            return leftResult > rightResult ? leftResult : rightResult;
        } else {// 无此节点，返回0
            return 0;
        }
    }
}
```

不过我们稍加思考一下，就可以进一步简略一下代码。因为我们代码里对于root为null的情况下返回的是0，那其实没有左节点时，对齐使用函数返回的也会是0，加1的话就是我们需要的1了，所以其实不用判断有无左节点，右节点也是一样。所以可以简化如下：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int maxDepth(TreeNode root) {
        if (root != null) {
	        int rightResult = maxDepth(root.left) + 1;
	        int leftResult = maxDepth(root.right) + 1;
            return leftResult > rightResult ? leftResult : rightResult;
        } else {
            return 0;
        }
    }
}
```

[回到目录](#Catalogue)

-------------------------

更多内容查看[我的博客](http://blog.csdn.net/column/details/cloudox-column2.html)
