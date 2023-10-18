指针
=======

为了在第一个输入之外的其它聪进行铭刻，可以提供一个被称为”指针”的从零开始的整数，标签为`2`，这会导致在输出的给定位置的聪上进行铭刻。如果指针等于或大于铭文交易输出中的总聪量，它就会被忽视，然后将按照通常的方式铭刻。指针字段的值是一个小端整数，尾部的零被忽视。

使用偶数标签，这样旧版本的 `ord` 会将铭文视为未绑定，而不是错误地将其分配给第一个聪。

这可以用于在单个交易中在不同的聪创建多个铭文，否则它们将在同一个聪上进行。

示例
--------

带有指针 255 的铭文：

```
OP_FALSE
OP_IF
  OP_PUSH "ord"
  OP_PUSH 1
  OP_PUSH "text/plain;charset=utf-8"
  OP_PUSH 2
  OP_PUSH 0xff
  OP_PUSH 0
  OP_PUSH "Hello, world!"
OP_ENDIF
```

带有指针 256 的铭文：

```
OP_FALSE
OP_IF
  OP_PUSH "ord"
  OP_PUSH 1
  OP_PUSH "text/plain;charset=utf-8"
  OP_PUSH 2
  OP_PUSH 0x0001
  OP_PUSH 0
  OP_PUSH "Hello, world!"
OP_ENDIF
```

带有指针 256 的铭文，末尾的零会被忽略：

```
OP_FALSE
OP_IF
  OP_PUSH "ord"
  OP_PUSH 1
  OP_PUSH "text/plain;charset=utf-8"
  OP_PUSH 2
  OP_PUSH 0x000100
  OP_PUSH 0
  OP_PUSH "Hello, world!"
OP_ENDIF
```