指针
=======

为了在除第一个输入之外的卫星上进行铭记，可以提供一个从零开始的整数，称为"指针"，并使用标签`2`，使得铭记在指定位置的卫星上进行。如果指针等于或大于铭记交易输出中的卫星总数，则会被忽略，并且铭记将按照通常的方式进行。指针字段的值是一个小端整数，末尾的零会被忽略。

使用偶数标签，这样旧版本的`ord`会将铭记视为未绑定，而不是错误地将其分配给第一个卫星。

这可以用于在单个交易中在不同的卫星上创建多个铭记，否则它们将在同一个卫星上进行。

示例
--------

带有指针 255 的铭记：

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

带有指针 256 的铭记：

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

带有指针 256 的铭记，末尾的零会被忽略：

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