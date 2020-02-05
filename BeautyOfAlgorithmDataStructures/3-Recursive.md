# 3-递归

递归求解问题的分解过程，去的过程叫“递”，回来的过程叫“归”。基本上，所有的递归问题都可以用递推公式来表示。

##### 递归需要满足的三个条件

- 一个问题的解可以分解为几个子问题的解。
- 这个问题与分解之后的子问题，除了数据规模不同，求解思路完全一样*。
- 存在递归终止条件。

**重点是写出递推公式，找到终止条件。**

##### 小试牛刀

假如这里有 n 个台阶，每次你可以跨 1 个台阶或者 2 个台阶，请问走这 n 个台阶有多少种走法？如果有 7 个台阶，你可以 2，2，2，1 这样子上去，也可以 1，2，1，1，2 这样子上去，总之走法有很多，那如何用编程求得总共有多少种走法呢？

> 递推公式 f(n) = f(n-1) + f(n-2)
> 终止条件 f(2)= 2，f(1)= 1;

##### 注意事项

试图想清楚整个递和归过程的做法，实际上是进入了一个思维误区。如果一个问题 A 可以分解为若干子问题 B、C、D，你可以假设子问题 B、C、D 已经解决，在此基础上思考如何解决问题 A。而且，你只需要思考问题 A 与子问题 B、C、D 两层之间的关系即可，不需要一层一层往下思考子问题与子子问题，子子问题与子子子问题之间的关系。屏蔽掉递归细节，这样子理解起来就简单多了。

##### 递推的弱点

- 堆栈溢出，如果最大深度比较小，比如 10、50，就可以用这种方法，否则这种方法并不是很实用。
- 重复计算，为了避免重复计算，我们可以通过一个数据结构（比如散列表）来保存已经求解过的 f(k)。
- 过多的函数调用会耗时较多等问题。

##### 如何更改递归为非递归

**覃超**老师的说法，**递推**。

使用 迭代循环 手动模拟本有系统完成的入栈和出栈操作。