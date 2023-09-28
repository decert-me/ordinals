序数浏览器
================

`ord` 文件包含一个区块浏览器。我们的主网区块链器部署在 [ordinals.com](https://ordinals.com) ， signet 部署在 [signet.ordinals.com](https://signet.ordinals.com)。



### 运行浏览器

服务器可以使用本地运行：

`ord server`

指定端口使用 `--http-port` 标记

`ord server --http-port 8080`

测试你的铭文你可以运行：

`ord preview <FILE1> <FILE2> ...`

## 搜索

搜索框可以使用各种对象：
Search

### 区块

区块可以通过哈希来查找，例如创世区块：

[000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f](https://ordinals.com/search/000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f)



### 交易

可以通过哈希查找交易，例如创世区块的 coinbase 交易：


[4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b](https://ordinals.com/search/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b)

### 输出

可以通过 outpoint 搜索交易输出，例如创世块 coinbase 交易的唯一输出：


[4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b:0](https://ordinals.com/search/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b:0)

### 聪

聪 可以按整数搜索，它们在整个比特币供应中的位置：

[2099994106992659](https://ordinals.com/search/2099994106992659)

按十进制，它们的块和该块内的偏移量：

[481824.0](https://ordinals.com/search/481824.0)

按度数，他们的周期，自上次减半以来的区块，自上次难度调整以来的区块，以及区块内的偏移量：

[1°0′0″0‴](https://ordinals.com/search/1°0′0″0‴)

按照名称，它们使用字母 "a" 到 "z" 的 26 个字母组合表示：

[ahistorical](https://ordinals.com/search/ahistorical)

或者按百分位数，在开采时已经或将要发行的比特币供应量的百分比：

[100%](https://ordinals.com/search/100%)
