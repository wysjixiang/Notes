### 一.数据结构

1. 简单点的，主要需要熟悉以下容器：
   1. vector向量容器，和树组类似，但更方便
   2. stack，栈，先进后出
   3. queue，先进先出队列
   4. deque，两端队列，这个容器在实现特定算法时很有用，比如滑动窗口最大值
2. 熟悉链表，熟悉链表反转算法： 主要只需要三个指针，前指针，当前指针，后指针，很简单
3. 哈希表：搞懂怎么用就行了！在复制random链表的那个题目里面有使用，大概使用方法如下：
   1. unordered_map<int, int> map;
   2. 当使用map[int ]索引时，就能索引到他单向指向的int值。
   3. 比如map[a] = b;  数据类型不局限于int，甚至可以是指针等等
4. 堆
   1. 小顶堆：每个结点的值都小于或等于其左右孩子结点的值，即顶节点最小
   2. 大顶堆：顶节点最大
   3. 使用优先队列来实现大小顶堆，priority_queue<Type, Container, Functional>
   4. 如大顶堆实现： priority_queue<int, vector<int>,  less>，默认情况就是大顶堆
   5. 注意优先队列的less和greater和一般的不同，less表示从大到小排序，优先输出最大的数，而如果是vector中使用less，则是从小到大排序的。这和底层实现的数据结构有关系！




### 二.动态规划

1. 动态规划主要思路就是求递推关系！！！寻找后面数据与前面数据的重要关系，然后能找出递推式解决！
2. 经典问题，形如斐波那契数列，青蛙跳台问题！
3. 正则表达式匹配问题，就多了一些判断条件，对于"*", "."这些，需要分情况讨论
4. 连续子数组的最大和问题，也是递推关系的一种，即dp[i-1]和dp[i]的关系。
5. n个骰子的点数问题，还利用了分治的思想，把n个骰子和n-1个骰子的关系联系起来，从n-1个骰子的式子出发推导



