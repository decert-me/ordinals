溯源
==========

铭文的所有者可以创建子铭文，在链上无需信任地建立这些子铭文的'溯源'，证明它们是由父铭文的所有者创建的。这可以用于集合，父铭文的子铭文会成为同一集合的成员。

子铭文自己也可以有子铭文，从而形成复杂的层级结构。
例如，一位艺术家可能创建一个代表自己的铭文，子铭文代表他们创建的合辑，而那些子铭文的子项就是合辑中的项目。



### 规范

为父系铭文 P 创建一个子铭文 C：

- 像通常一样为 C 创建常用的铭刻交易 T。
- 在其中的一个 T 输入中加入父系铭文 P
- 在 C 中包含标签 `3`，即 `OP_PUSH 3`，其值为 P 的序列化二进制铭文 ID 序列化为 32 字节的 `TXID`，
后跟四字节的小端 `INDEX`，不含末尾的零。



*请注意*，比特币交易 ID 的字节在文本中的表现形式是反向的，所以序列化的交易 ID 会以相反的顺序呈现。

### 示例

子铭文的一个示例
`000102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1fi0`:


```
OP_FALSE
OP_IF
  OP_PUSH "ord"
  OP_PUSH 1
  OP_PUSH "text/plain;charset=utf-8"
  OP_PUSH 3
  OP_PUSH
0x1f1e1d1c1b1a191817161514131211100f0e0d0c0b0a09080706050403020100
  OP_PUSH 0
  OP_PUSH "Hello, world!"
OP_ENDIF
```




请注意，标签 `3` 的值是二进制的，而不是十六进制的，主要是为了让子铭文识别出来是个子铭文，
`000102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1fi0` 必须作为铭文交易的输入之一




铭文 ID 的编码示例
`000102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1fi255`:


```
OP_FALSE
OP_IF
  …
  OP_PUSH 3
  OP_PUSH
0x1f1e1d1c1b1a191817161514131211100f0e0d0c0b0a09080706050403020100ff
  …
OP_ENDIF
```



以及铭文 ID
`000102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1fi256`:

```
OP_FALSE
OP_IF
  …
  OP_PUSH 3
  OP_PUSH
0x1f1e1d1c1b1a191817161514131211100f0e0d0c0b0a090807060504030201000001
  …
OP_ENDIF
```



### 注释

标签 `3` 被使用是因为它是第一个可用的奇数标签。
未识别的奇数标签不会使铭文无法进行绑定，因此，旧版本的 ord 仍可以识别和追踪子铭文。

通过销毁集合的父铭文，可以关闭一个集合，这保证了该集合中不能再发行更多的项目。

