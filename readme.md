# 批量重命名工具

## 使用示例

```bash
# 给当前目录所有文件按文件名升序排序，添加阿拉伯数字序号作为前缀
rename_tool.py -m name -o asc -c 1 -t num -n "%d_%s"

# 使用正则匹配文件名，按修改时间降序排序，数字转中文，添加后缀
rename_tool.py -r -m mtime -o desc -t chinese -p "文件_" -s "_备份" ".*\.txt$"

# 将文件名中的数字替换为罗马数字
rename_tool.py -N "\d+" roman

# 删除文件名前缀"test_"，修改扩展名为".md"
rename_tool.py -p "test_" -e md

# 对文件名做切片，只保留第2到第5个字符
rename_tool.py -S 1 5

```

## 参数说明

| 参数                  | 说明                                                                               | 示例             |
| --------------------- | ---------------------------------------------------------------------------------- | ---------------- |
| `-r, --regexp`        | 使用正则表达式匹配文件名                                                           | `-r`             |
| `filenames`           | 指定文件名或通配符，默认当前目录所有文件                                           | `*.txt`          |
| `-c, --count`         | 排序数字起始值，默认 0                                                             | `-c 1`           |
| `-T, --step`          | 排序数字步长，默认 1                                                               | `-T 2`           |
| `-t, --type`          | 数字格式，支持 `num`（阿拉伯）、`chinese`、`english`、`roman`、`fillzero`          | `-t chinese`     |
| `-m, --method`        | 排序方式，支持 `name`、`ctime`（创建时间）、`mtime`(修改时间)、`size`、`extension` | `-m mtime`       |
| `-o, --order`         | 排序顺序，`asc` 升序（默认），`desc` 降序                                          | `-o desc`        |
| `-n, --newname`       | 新文件名模板，`%d`为数字，`%s`为原文件名                                           | `-n "第%d个_%s"` |
| `-e, --extension`     | 修改文件扩展名                                                                     | `-e jpg`         |
| `-s, --suffixdelete`  | 删除文件名前缀                                                                     | `-s "_old"`      |
| `-p, --prefixdelete`  | 删除文件名前缀                                                                     | `-p "test_"`     |
| `+s, --suffixadd`     | 添加后缀，不含扩展名                                                               | `+s "_backup"`   |
| `+p, --prefixadd`     | 添加前缀                                                                           | `+p "new_"`      |
| `-d, --delete`        | 删除文件名中指定字符串                                                             | `-d "temp"`      |
| `-a, --add`           | 替换文件名字符串，两个参数：原字符串 新字符串                                      | `-a old new`     |
| `-R, --regexpreplace` | 正则替换，两个参数：正则表达式 替换字符串                                          | `-R "\d+" "NUM"` |
| `-N, --numreplace`    | 替换数字，两个参数：正则表达式 数字格式（`chinese`等）                             | `-N "\d+" roman` |
| `-S, --slice`         | 切片操作，1 或 2 个整数参数，字符串切片区间                                        | `-S 0 3`         |

## 数字格式说明

- num：阿拉伯数字，例如 1, 2, 3

- chinese：中文数字，例如 一, 二, 三

- roman：罗马数字，例如 I, II, III

- fillzero：阿拉伯数字补零，位数自动补齐，例如 001, 002, 003

## 注意事项

- 正则表达式匹配时，参数只取第一个作为匹配规则

- 切片操作区间超过字符串长度时，会自动截断

- 替换和删除操作均针对文件名（不含拓展名）

- 重命名前请确保不会造成文件名冲突，脚本默认直接覆盖
