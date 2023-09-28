重新索引
==========

有时必须重新索引‘ord’数据库，这意味着删除数据库并使用 `ord index update` 或 `ord server` 来重新索引数据库。重新索引的原因是：

1. ord 发布新的主要版本，更改了数据库架构
2. 数据库可能会损坏

`ord` 使用的数据库称为 [redb](https://github.com/cberner/redb)，所以我们为索引指定默认文件名‘index.redb’。默认情况下我们存储根据您的操作系统，此文件位于不同的位置。



|Platform | Value                                            | Example                                      |
| ------- | ------------------------------------------------ | -------------------------------------------- |
| Linux   | `$XDG_DATA_HOME`/ord or `$HOME`/.local/share/ord | /home/alice/.local/share/ord                 |
| macOS   | `$HOME`/Library/Application Support/ord          | /Users/Alice/Library/Application Support/ord |
| Windows | `{FOLDERID_RoamingAppData}`ord                  | C:UsersAliceAppDataRoamingord           |

因此，要在 MacOS 上删除数据库并重新索引，您必须在终端中执行以下命令：


```bash
rm ~/Library/Application Support/ord/index.redb
ord index update
```


您当然也可以自己设置数据目录的位置,`ord --data-dir <DIR> index update` 或为其指定特定的文件名和路径, 使用‘ord --index <FILENAME /> 索引运行’。

