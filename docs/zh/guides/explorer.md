序数浏览器
================

`ord` 文件包含一个区块浏览器。我们的主网区块链器部署在 [ordinals.com](https://ordinals.com) ， signet 部署在 [signet.ordinals.com](https://signet.ordinals.com)。



### 运行浏览器

服务器可以使用本地运行：

`ord server`

指定端口使用 `--http-port` 标记

`ord server --http-port 8080`

要启用 JSON-API 端点，请添加`--enable-json-api`或`-j`标志（有关更多信息，请参见[这里](#json-api)）：

`ord --enable-json-api server`

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

JSON-API
--------

您可以使用`ord`命令并添加`--enable-json-api`标志来访问返回 JSON 而不是 HTML 的端点，如果您设置了 HTTP `Accept: application/json`头。这些对象的结构与 HTML 中显示的内容非常相似。这些端点包括：

- `/inscription/<INSCRIPTION_ID>`
- `/inscriptions`
- `/inscriptions/block/<BLOCK_HEIGHT>`
- `/inscriptions/block/<BLOCK_HEIGHT>/<PAGE_INDEX>`
- `/inscriptions/<FROM>`
- `/inscriptions/<FROM>/<N>`
- `/output/<OUTPOINT>`
- `/output/<OUTPOINT>`
- `/sat/<SAT>`

要获取最新的 100 个铭文的列表，您可以执行以下操作：

```
curl -s -H "Accept: application/json" 'http://0.0.0.0:80/inscriptions'
```

要查看包含其中铭文的 UTXO 的信息，请执行以下操作：

```
curl -s -H "Accept: application/json" 'http://0.0.0.0:80/output/bc4c30829a9564c0d58e6287195622b53ced54a25711d1b86be7cd3a70ef61ed:0'
```

返回结果如下：

```
{
  "value": 10000,
  "script_pubkey": "OP_PUSHNUM_1 OP_PUSHBYTES_32 156cc4878306157720607cdcb4b32afa4cc6853868458d7258b907112e5a434b",
  "address": "bc1pz4kvfpurqc2hwgrq0nwtfve2lfxvdpfcdpzc6ujchyr3ztj6gd9sfr6ayf",
  "transaction": "bc4c30829a9564c0d58e6287195622b53ced54a25711d1b86be7cd3a70ef61ed",
  "sat_ranges": null,
  "inscriptions": [
    "6fb976ab49dcec017f1e201e84395983204ae1a7c2abf7ced0a85d692e442799i0"
  ]
}
```