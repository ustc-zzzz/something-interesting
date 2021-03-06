# 第一关题解

首先，打开谜题页面<https://ustc-zzzz.github.io/something-interesting/>后，通过对URL的分析，可以注意到的一点是，页面自动加上了一个`#1`的后缀，变成了<https://ustc-zzzz.github.io/something-interesting/#1>，然后通过点击其中的链接，相应的后缀会发生改变（`?preview=1#42`），通过不停地迭代，可以形成一条链：`1, 42, 38, 169, ...`。

这条链也可以直接通过查看存储数据的JS（[data.js](./data.js)）得到。通过观察可以发现的是，这条链形成了一个环，所有的数据都在0（包含）和254（包含）之间，但这个环却只覆盖了240个数。

剩下的15个数便组成了一个列表：

`7 24 41 58 75 92 109 126 143 160 177 194 211 228 245`

我们从这个列表的任何一个元素开始，都能够注意到，整个列表中的每一个元素组合起来，会形成一个环：

`24, 245, 211, 177, 143, 109, 75, 41, 7, 228, 194, 160, 126, 92, 58, 24, ...`

访问相应的网页，把数字和字符对应起来：

`uationlinear_equ...`

调整一下字母的顺序：

`linear_equation`

便是本关的答案（线性方程）。

# 第一关题解补充

这一答案和本关题目的设置其实也有关系，因为这条迭代链，是通过一种名为[线性同余方法](https://en.wikipedia.org/wiki/Linear_congruential_generator)的方式生成出来的。线性同余方法需要指定一个用于迭代的线性方程，针对本关的线性方程就是：

`x(n + 1) = (31 * x(n) + 11) % 255`

因为取余操作的存在，可以保证的是，所有的数据都在0（包含）和254（包含）之间。由于每个数字的生成都只和上一个数字的生成有关，而可用的数字又是有限的，所以一定会出现循环，而对于本关而言，相应的循环就有两个，一个的周期为240，另一个的为15。

所以正确答案就是那个循环周期为15的序列里，而剩下的240个位置的字符，就直接使用随机的[Base64](https://en.wikipedia.org/wiki/Base64)字符填充了。

P.S. 还有一点，为了凑整，255对应的字符也被我填充了，和0对应的是一样的。
