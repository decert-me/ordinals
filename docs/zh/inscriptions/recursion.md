---
title: "DeCert.Me | undefined"
description: "undefined"
image: "https://ipfs.decert.me/undefined"
sidebar_label: "递归"
---
递归
=========

[沙盒化](../inscriptions.md#sandboxing)的一个重要例外是递归：访问“ord”的“/
content”允许端点，允许铭文访问其他端点的内容通过请求 `/content/
<INSCRIPTION_ID>` 来获取铭文。




这有许多有趣的用例：

- 重新混合现有铭文的内容。

- 将代码、图像、音频或样式表片段发布为公共的共享资源。


- 生成艺术收藏，其中算法使用JavaScript刻写，并从具有独特种子的多个铭文中实例
化。


- 生成个人资料图片集，其中包含配件和属性刻录为单独的图像，或刻录在共享纹理图集
中，然后组合，拼贴风格，在多个铭文中以独特的组合。



铭文可以访问的其他几个端点如下：

- `/blockheight`：最新区块高度。
- `/blockhash`：最新的块哈希。
- `/blockhash/<HEIGHT>`：给定块高度的块哈希。
- `/blocktime`：最新块的 UNIX 时间戳。
