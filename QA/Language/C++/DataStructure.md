# DataStructure

### 术语
节点的层次
> 从根开始定义,根为第1层,根的子节点为第二层,以此类推

深度
> 对于任意节点n,n的深度为从根到n的唯一**路径**长,根的深度为0

高度
> 对于任意节点n,n的高度为从n到**一片树叶**的最长路径长,所有树叶的深度为0

### 无根树
一个没有固定根节点的树称为无根树
  * 有n个结点,n-1条边的连通无向图
  * 无向无环的连通图
  * 任意两个节点之间有且仅有一条简单路径的无向图
  * 任何边均为桥的连通图
### 有根图
在无根树的基础上,指定一个结点称为根,形成一棵有根树

### B树
[wiki](https://zh.wikipedia.org/wiki/B%E6%A0%91)

B树是一种自平衡的数,能够保持数据有序,且令增删查改操作能在对数时间内完成
,与自平衡二叉查找树不同,B树适合于读写相对大的数据块的存储系统
##### 内部
B树中:
* 内部(非叶子)节点可以拥有可变数量的子节点(数量范围预先定义好)
* 当数据被插入或删除时, 为维持预先设定的数量范围内,内部节点可能会被合并或分离
* B树通过约束所有叶子节点在相同深度保持平衡
* B树在它内部节点中存储键值,但不需在叶子节点上存储这些键值记录
* 每一个内部节点会包含一定数量的键,键将节点的子树分开.通常键的数量被选定在d和2d之间(d是键的最小数量). 
  * 当内部的节点有2d个键的时候,给节点添加一个键,将这2d+1个键拆分成2个包含d个键的节点并将中间节点移动到父节点.
  * 当两个相邻的节点A,B均有d个键,在A节点删除1个键后和B节点以及它们的父节点合并,最后得到一个拥有2d键的节点

##### 定义
一个m阶的B数:
* 每一个节点最有有m个子节点
* 每一个非叶子节点(除根节点外)最少有m/2个子节点
* 如果根节点不是叶子节点,那么它至少有两个子节点
* 有k个子节点的非叶子节点拥有k-1个键
* 所有叶子节点都在同一层

内部节点:
* 除叶子节点和根节点外的所有节点,通常表示为一组有序的元素和指向子节点的指针
* 每一个内部节点拥有最多U个,最少L个子节点,满足$U=2L或U=2L-1$
  > U和L之间的关系意味这两个半满的子节点可以合并成一个合法的节点,一个全满的节点可以被分裂成两个合法的节点(前提父节点有空间容纳多一个元素)

根节点:
  * 根节点拥有的节点数量上限和内部节点相同,但没有下限

叶子节点:
  *  叶子节点对元素的数量有相同的限制

##### 算法
搜索:
  * 类似二叉树搜索

插入:
  * 如果节点拥有的元素数量小于最大值,那么将元素插入并保持有序
  * 如果插入后节点的元素已经满,就分裂
    * 从当前节点中选出中位数
    * 小于中位数的元素放在左边节点,大于中位数的放在右边节点
    * 中位数被插入到父节点,

删除:  
  * 删除叶子节点中的元素
    1. 搜索要删除的元素
    2. 如果是叶子节点,将它直接删除
    3. 如果产生下溢出,进行平衡
  * 删除内部节点中的元素
    1. 选择一个新的分隔符(左子树中最大的元素或右子树中最小的元素),将它从叶子节点中移除,替换被删除的元素作为新的分隔值
    2. 被删除元素的子节点如果元素数小于最低要求,那么需要重新平衡
  * 平衡
    * 如果缺少元素节点的有兄弟存在且拥有多余的元素,那么左旋转
      1. 将父节点的分隔符复制到缺少元素节点的最后
      2. 将父节点的分隔值替换为右兄弟节点的第一个元素
    * 如果缺少元素节点的左兄弟存在且拥有多余的元素,那么右旋转
    * 如果两个直接的兄弟都无多余的元素,将它与一个直接兄弟节点以及父节点中它们的分隔值合并
      * 将分隔值复制到左边的节点(可能是缺少元素或者是拥有最小数量元素的兄弟节点)
      * 将右边节点中所有的元素移动到左边节点,此时左边节点拥有最大数量元素,右边节点为空
      * 将父节点中的分隔值和空的右子树移除
        * 如果父节点是根节点并且没有元素了,那么释放它并由合并后的节点称为新根节点
        * 如果父节点元素小于最小值,重新平衡父节点

### B+树
B+树是B树的一个升级班,相对于B树而言,B+数充分利用了节点空间,查询速度更快
能够保持数据的稳定有序，通常用于数据库和操作系统的文件系统
##### 节点结构
* B+树的非叶子节点不保存关键字记录的指针,只进行数据索引
* B+树的叶子节点保存了父节点的所有关键字记录的指针,所有数据地址必须要到叶子节点才能获得,每次查询数据的次数是一样
* B+树叶子节点的关键字从小到大有序排列,左边结尾数据都会保存右边节点开始数据的指针
* 非叶子节点的子节点数 = 关键字数 或者 非叶节点的关键字数=子节点数-1

### 并查集
[wiki](https://zh.wikipedia.org/wiki/%E5%B9%B6%E6%9F%A5%E9%9B%86)
并查集是一种树型的数据结构,用于处理一些不交集的合并及查询问题

waiting
