猎聪
===========

*本指南已过时。自编写以来，“ord” 安装文件已更改仅当提供 “--index-sats” 标志时才构建完整的聪索引。此外，“ord” 现在有一个内置钱包，其中包含比特币核心钱包。请参阅 `ord wallet --help`。*

Ordinal 寻找是困难但有回报的。拥有一只装满 UTXOs 的钱包，充满了稀有和奇特的聪的气味，是无与伦比的。

序数是 satoshi 的数字。每个 satoshi 都有一个序数，每个序数都有一个 satoshi。

准备工作
-----------

在开始之前，你需要几样东西。

1. 首先，你需要一个已同步的 Bitcoin Core 节点，并带有事务索引。要打开事务索引，请在命令行中传递`-txindex`：

```sh
bitcoind -txindex
```

或者将以下内容放入你的[Bitcoin 配置文件](https://github.com/bitcoin/bitcoin/blob/master/doc/bitcoin-conf.md#configuration-file-path)：

```
txindex=1
```

启动它并等待它更新到最新链上数据，此时以下命令应打印出当前块高度：

```sh
bitcoin-cli getblockcount
```

2. 其次，你需要一个已同步的`ord`索引。
   - 从 [repo](https://github.com/ordinals/ord/) 获取 `ord` 的副本。
   - 运行 `RUST_LOG=info ord index`。它应连接到你的 Bitcoin Core 节点并开始索引。
   - 等待它完成索引。

3. 第三，你需要一个带有你要搜索的 UTXOs 的钱包。

搜索稀有序数
---------------------------

#### 在 Bitcoin Core 钱包中搜索稀有序数

`ord wallet` 命令只是 Bitcoin Core 的 RPC API 的包装器，因此在 Bitcoin Core 钱包中搜索稀有序数很容易。假设你的钱包名为 `foo`：

1. 加载你的钱包：

  ```sh
  bitcoin-cli loadwallet foo
  ```

2. 显示钱包 `foo` 的任何稀有序数的 UTXOs：

  ```sh
  ord wallet sats
  ```

#### 在非 Bitcoin Core 钱包中搜索稀有序数

`ord wallet`命令只是 Bitcoin Core 的 RPC API 的包装器，因此要在非 Bitcoin Core 钱包中搜索稀有序数，你需要将你的钱包描述符导入到 Bitcoin Core 中。

[描述符](https://github.com/bitcoin/bitcoin/blob/master/doc/descriptors.md)描述了钱包生成私钥和公钥的方式。

你应该只将公钥描述符导入到比特币核心中，而不是私钥描述符。

如果你的钱包的公钥描述符被攻击者获取，攻击者将能够看到你的钱包地址，但你的资金将是安全的。

如果你的钱包的私钥描述符被攻击者获取，攻击者可以将你的钱包中的资金全部转走。

1. 从你想要搜索稀有序号的 UTXOs 的钱包中获取钱包描述符。它会看起来像这样：

  ```
  wpkh([bf1dd55e/84'/0'/0']xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/0/*)#csvefu29
  ```

2. 创建一个只读钱包，命名为 `foo-watch-only`：

  ```sh
  bitcoin-cli createwallet foo-watch-only true true
  ```

请随意给它一个比 `foo-watch-only` 更好的名字！

3. 加载 `foo-watch-only` 钱包：

  ```sh
  bitcoin-cli loadwallet foo-watch-only
  ```

4. 将你的钱包描述符导入到 `foo-watch-only` 中：

  ```sh
  bitcoin-cli importdescriptors
    '[{"desc":
  "wpkh([bf1dd55e/84h/0h/0h]xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/0/*)#tpnxnxax", "timestamp":0 }]'
  ```

  如果你知道你的钱包首次开始接收交易的 Unix 时间戳，你可以将其用作 `"timestamp"` 的值，而不是`0`。这将减少比特币核心搜索你的钱包 UTXOs 所需的时间。

5. 检查一切是否正常：

  ```sh
  bitcoin-cli getwalletinfo
  ```

6. 显示你的钱包的稀有序号：

  ```sh
  ord wallet sats
  ```

#### 在导出多路径描述符的钱包中搜索稀有序号

一些描述符使用尖括号来描述一个描述符中的多个路径，例如`<0;1>`。比特币核心尚不支持多路径描述符，因此你首先需要将它们转换为多个描述符，然后将这些多个描述符导入到比特币核心中。

1. 首先从你的钱包中获取多路径描述符。它会看起来像这样：

  ```
  wpkh([bf1dd55e/84h/0h/0h]xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/<0;1>/*)#fw76ulgt
  ```

2. 为接收地址路径创建一个描述符：

  ```
  wpkh([bf1dd55e/84'/0'/0']xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/0/*)
  ```

  以及更改地址路径：

  ```
  wpkh([bf1dd55e/84'/0'/0']xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/1/*)
  ```

3. 获取并记录接收地址描述符的校验和，本例中为 `tpnxnxax`:

  ```sh
  bitcoin-cli getdescriptorinfo
    'wpkh([bf1dd55e/84h/0h/0h]xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/0/*)'
  ```

  ```json
  {
    "descriptor":
  "wpkh([bf1dd55e/84'/0'/0']xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/0/*)#csvefu29",
    "checksum": "tpnxnxax",
    "isrange": true,
    "issolvable": true,
    "hasprivatekeys": false
  }
  ```

  以及更改地址描述符的校验和，本例中为 `64k8wnd7`:

  ```sh
  bitcoin-cli getdescriptorinfo
    'wpkh([bf1dd55e/84h/0h/0h]xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/1/*)'
  ```

  ```json
  {
    "descriptor":
  "wpkh([bf1dd55e/84'/0'/0']xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/1/*)#fyfc5f6a",
    "checksum": "64k8wnd7",
    "isrange": true,
    "issolvable": true,
    "hasprivatekeys": false
  }
  ```

4. 加载要导入描述符的钱包：

  ```sh
  bitcoin-cli loadwallet foo-watch-only
  ```

5. 现在将带有正确校验和的描述符导入到比特币核心中。

  ```sh
  bitcoin-cli
  importdescriptors
  '[
    {
      "desc":
  "wpkh([bf1dd55e/84h/0h/0h]xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/0/*)#tpnxnxax"
      "timestamp":0
    },
    {
      "desc":
  "wpkh([bf1dd55e/84h/0h/0h]xpub6CcJtWcvFQaMo39ANFi1MyXkEXM8T8ZhnxMtSjQAdPmVSTHYnc8Hwoc11VpuP8cb8JUTboZB5A7YYGDonYySij4XTawL6iNZvmZwdnSEEep/1/*)#64k8wnd7",
      "timestamp":0
    }
  ]'
  ```

  如果你知道钱包首次开始接收交易的 Unix 时间戳，可以将其用作 `"timestamp"` 字段的值，而不是 `0`。这将减少比特币核心搜索钱包的未使用交易输出所需的时间。

6. 检查是否一切正常：

  ```sh
  bitcoin-cli getwalletinfo
  ```

7. 显示钱包的稀有序数：

  ```sh
  ord wallet sats
  ```

### 导出描述符

#### Sparrow 钱包

导航到 `设置` 选项卡，然后到 `脚本策略`，点击编辑按钮以显示描述符。

### 转移序数

`ord` 钱包支持转移特定的聪。你还可以使用 `bitcoin-cli` 命令 `createrawtransaction`、`signrawtransactionwithwallet` 和 `sendrawtransaction`，但这样做比较复杂，超出了本指南的范围。



