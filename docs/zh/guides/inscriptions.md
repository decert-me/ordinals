铭文指引
=========================

单个 聪 可以刻有任意内容，创建可以保存在比特币钱包中并使用比特币交易传输的比特币原生数字人工制品。铭文与比特币本身一样持久、不变、安全和去中心化。




使用铭文需要一个比特币完整节点，让您了解比特币区块链的当前状态，以及一个可以创建铭文并在构建交易以将铭文发送到另一个钱包时执行 聪 控制的钱包。




Bitcoin Core 提供比特币全节点和钱包。 但是，Bitcoin Core 钱包不能创建铭文，不执行 聪 控制。


这需要 [`ord`](https://github.com/ordinals/ord)，序数实用程序。 `ord` 没有自己的钱包，因此  `ord wallet` 子命令与 Bitcoin Core 钱包交互。



本指南涵盖：

1. 安装 Bitcoin Core
2. 同步比特币区块链
3. 创建 Bitcoin Core 钱包
4. 使用 `ord wallet receive` 收取聪
5. 使用 `ord wallet inscribe` 创建铭文
6. 使用 `ord wallet send` 发送铭文
7. 使用 `ord wallet receive` 收取铭文

寻求帮助
------------

如果你遇到困难，可以在 [Ordinals Discord Server](https://discord.com/invite/87cjuz4FYg), 或者检查 Github 上的相关内容 [问题](https://github.com/ordinals/ord/issues) 和 [讨论](https://github.com/ordinals/ord/discussions)。




安装 Bitcoin Core
-----------------------

Bitcoin Core 可以在 [bitcoincore.org](https://bitcoincore.org/) 上的 [下载页面](https://bitcoincore.org/en/download/)。


制作铭文需要 Bitcoin Core 24 或者更新版本。

本指南不详细介绍安装比特币核心的步骤。一旦安装完成比特币核心，你应该能够在命令行中成功运行 `bitcoind -version` 。请 *不要* 使用 `bitcoin-qt` 。

配置 Bitcoin Core
------------------------

`ord` 需要 Bitcoin Core 的交易索引和 REST 接口。

`ord` 需要 Bitcoin Core 的交易索引配置你的 Bitcoin Core 阶段去维护一个交易索引，需要在 `bitcoin.conf` 里面添加：


```
txindex=1
```



或者, 运行 `bitcoind` 和 `-txindex`：

```
bitcoind -txindex
```

有关创建或修改 `bitcoin.conf` 文件的详细信息可以在 [这里](https://github.com/bitcoin/bitcoin/blob/master/doc/bitcoin-conf.md) 找到。

比特币区块同步
------------------------------

区块同步，运行：

```
bitcoind -txindex
```

… 直到运行 `getblockcount` ：

```
bitcoin-cli getblockcount
```


像区块链浏览器 [the mempool.space block explorer](https://mempool.space/) 一样对区块进行记述。 `ord` 同 `bitcoind` 进行交互, 所以你在使用 `ord` 时候需要让 `bitcoind` 在后台运行。

区块链占用约 600GB 的磁盘空间。如果你有一个外部驱动器想要存储区块，可以使用配置选项 `blocksdir=<external_drive_path>` 。这比使用 `datadir` 选项要简单得多，因为 cookie 文件仍将位于 `bitcoin-cli` 和 `ord` 默认位置以供查找。

故障排除
---------------

确保你可以通过 `bitcoin-cli -getinfo` 访问 `bitcoind` 并且它已完全同步。

如果 `bitcoin-cli -getinfo` 返回 ` 无法连接到服务器 `，则表示 `bitcoind` 未运行。

确保在你的 `bitcoin.conf` 文件中 *未设置* `rpcuser`、`rpcpassword` 或 `rpcauth`。`ord` 需要使用 cookie 身份验证。确保你的比特币数据目录中有一个名为 `.cookie` 的文件。

如果 `bitcoin-cli -getinfo` 返回 `无法找到 RPC 凭据`，则必须指定 cookie 文件位置。
如果你使用自定义数据目录（指定 `datadir` 选项），则必须像这样指定 cookie 位置：
`bitcoin-cli -rpccookiefile=<your_bitcoin_datadir>/.cookie -getinfo`。
在运行 `ord` 时，你必须使用 `--cookie-file=<your_bitcoin_datadir>/.cookie` 指定 cookie 文件位置。

确保你的 `bitcoin.conf` 文件中 *未设置* `disablewallet=1`。如果 `bitcoin-cli listwallets` 返回 `找不到方法`，则表示钱包已禁用，你将无法使用 `ord`。

确保设置了 `txindex=1`。运行 `bitcoin-cli getindexinfo` 应该返回类似以下的内容：
```json
{
  "txindex": {
    "synced": true,
    "best_block_height": 776546
  }
}
```
如果它只返回 `{}`，则表示未设置 `txindex`。
如果它返回 `"synced": false`，则表示 `bitcoind` 仍在创建 `txindex`。在使用 `ord` 之前，请等待 `"synced": true`。

如果你设置了 `maxuploadtarget`，它可能会干扰 `ord` 索引的区块获取。请删除它或设置 `whitebind=127.0.0.1:8333`。


安装 `ord`
----------------

`ord` 程序使用 Rust 语言写成，可以从 [源码](https://github.com/ordinals/ord) 安装. 预制文件可以从 [版本发布页](https://github.com/ordinals/ord/releases) 下载。



你也可以在命令行中使用下面命令来安装最新的文件：

```sh
curl --proto '=https' --tlsv1.2 -fsLS https://ordinals.com/install.sh | bash
-s
```



当 `ord` 成功安装以后, 你可以运行 ：
```
ord --version
```



这会返回 `ord` 的版本信息。

创建一个 Bitcoin Core 钱包
------------------------------

`ord` 使用 Bitcoin Core 来管理私钥，签署交易以及向比特币网络广播交易。


创建一个名为 `ord` 的 Bitcoin Core 钱包，运行：
```
ord wallet create
```


接收聪
--------------

铭文是在单个聪上制作的，使用聪来支付费用的普通比特币交易，因此你的钱包将需要一些 聪（比特币）。


为你的 `ord` 钱包创建一个新地址，运行：

```
ord wallet receive
```


向上面地址发送一些资金。

你可以使用以下命令看到交易情况：
```
ord wallet transactions
```


一旦交易确认，你应该可以使用 `ord wallet outputs` 看到交易的输出；


创建铭文内容
----------------------------

聪上可以刻录任何类型的内容，但 `ord` 钱包只支持 `ord` 区块浏览器可以显示的内容类型。


另外，铭文是包含在交易中的，所以内容越大，铭文交易需要支付的费用就越高。


铭文内容包含在交易见证中，获得见证折扣。要计算写入交易将支付的大概费用，请将内容大小除以四，然后乘以费率


铭文交易必须少于 400,000 个权重计量单位，否则不会被 Bitcoin Core 中继。一个字节的铭文内容需要一个权重计量单位。 由于铭文交易不只是铭文内容，铭文内容限制在 400,000 权重计量单位以内。390,000 个权重计量单位应该是安全的。


创建铭文
---------------------

以 `FILE` 的内容创建一个铭文，需要运行：

```
ord wallet inscribe --fee-rate FEE_RATE FILE
```


Ord 会输出两个交易 ID，一个是 commit 交易，一个是 reveal 交易，还有铭文 ID。铭文 ID 的格式为 `TXIDiN`，其中 `TXID` 是揭示交易的交易 ID，`N` 是揭示交易中铭文的索引。


Commit 交易提交到包含铭文内容的 tapscript，reveal 交易则从该 tapscript 中花费，显示链上的内容并将它们铭刻在 reveal 交易的第一个输出的第一个 sat 上。


在等待 reveal 交易被记录的同时，你可以使用 [the mempool.space block explorer](https://mempool.space/) 来检查交易的状态。

一旦 reveal 交易完成记账，你可以使用以下命令查询铭文 ID：


```
ord wallet inscriptions
```



父子铭文
-------------------------

父子铭文使得所谓的集合成为可能，详见[溯源](../inscriptions/provenance.md)以获取更多信息。

要将一个铭文设为另一个铭文的子铭文，父铭文必须已经被铭刻并存在于钱包中。要选择一个父铭文，请运行 `ord wallet inscriptions` 并复制铭文 ID（`<PARENT_INSCRIPTION_ID>`）。

现在，铭刻子铭文并指定父铭文，如下所示：

```
ord wallet inscribe --fee-rate FEE_RATE --parent <PARENT_INSCRIPTION_ID> CHILD_FILE
```

这种关系不能事后添加，父铭文必须在子铭文开始时存在。

发送铭文
----------------------

铭文接收方使用一下命令生成地址
```
ord wallet receive
```

使用命令格式发送铭文：
```
ord wallet send --fee-rate <FEE_RATE> <ADDRESS> <INSCRIPTION_ID>\n
```

检查未完成交易情况：
```
ord wallet transactions
```

一旦交易确认，接收方可以使用一下命令查看接收到的铭文
```
ord wallet inscriptions
```

接收铭文
------------------------

使用以下命令生成一个新的接收地址

```
ord wallet receive
```

发送方使用命令发送铭文到你的地址

```
ord wallet send ADDRESS INSCRIPTION_ID
```

检查未完成交易情况：
```
ord wallet transactions
```

一旦交易确认，你可以使用以下命令确认收到

```
ord wallet inscriptions
```
