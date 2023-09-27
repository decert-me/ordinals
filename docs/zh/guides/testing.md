调试
=======

使用以下标志来指定测试网络，可以测试 Ord。有关运行比特币核心进行测试的更多信息，请参见[比特币的开发者文档](https://developer.bitcoin.org/examples/testing)。

大多数在[铭文](inscriptions.md) 和 [浏览器](explorer.md) 中的 `ord`命令可以使用以下网络标志运行：


| Network | Flag |
|---------|------|
| Testnet | `--testnet` or `-t` |
| Signet  | `--signet` or `-s` |
| Regtest | `--regtest` or `-r` |

Regtest不需要下载区块链或者建立ord索引

示例
-------

在regtest里运行bitcoind，使用：
```
bitcoind -regtest -txindex
```


在regtest里创建钱包
```
ord -r wallet create
```


创建一个regtest接收地址
```
ord -r wallet receive
```


挖取101个区块（解锁coinbase）使用：
```
bitcoin-cli generatetoaddress 101 <receive address>
```


在regtest上铭刻
```
ord -r wallet inscribe --fee-rate 1 <file>
```


挖取铭文
```
bitcoin-cli generatetoaddress 1 <receive address>
```


在regtest浏览器里查看铭文
```
ord -r server
```



测试递归
-----------------

测试 [recursion](../inscriptions/recursion.md) 时，首先记下依赖项（以 [p5.js](https://p5js.org) 为例：

```
ord -r wallet inscribe --fee-rate 1 p5.js
```


这应该返回一个`inscription_id`，然后您可以在递归铭文中引用它。


请注意，在主网和signet上铭刻的时候这些id有所不同，因此请务必更改每个链的递归铭文中的内容。

现在你可以使用以下命令来铭刻你的递归铭文：
```
ord -r wallet inscribe --fee-rate 1 recursive-inscription.html
```


最终你可以挖取一些区块来开始服务器：
```
bitcoin-cli generatetoaddress 6 <receive address>
ord -r server
```



