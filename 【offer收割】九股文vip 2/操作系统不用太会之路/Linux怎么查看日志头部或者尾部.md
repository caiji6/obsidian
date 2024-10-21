# 👌Linux怎么查看日志头部或者尾部

### 查看日志文件头部
1. **使用**`**head**`**命令**：
    - `head`命令默认显示文件的前10行。
    - 示例：

```plain
head /path/to/logfile
```

+ 如果需要查看特定行数的头部内容，可以使用`-n`选项：

```plain
head -n 20 /path/to/logfile
```

+ 例如，查看`/var/log/syslog`的前20行：

```plain
head -n 20 /var/log/syslog
```

### 查看日志文件尾部
1. **使用**`**tail**`**命令**：
    - `tail`命令默认显示文件的后10行。
    - 示例：

```plain
tail /path/to/logfile
```

+ 如果需要查看特定行数的尾部内容，可以使用`-n`选项：

```plain
tail -n 20 /path/to/logfile
```

+ 例如，查看`/var/log/syslog`的后20行：

```plain
tail -n 20 /var/log/syslog
```

1. **实时查看日志文件**：
    - `tail`命令的`-f`选项可以用于实时查看日志文件的追加内容。
    - 示例：

```plain
tail -f /path/to/logfile
```

+ 例如，实时查看`/var/log/syslog`的内容：

```plain
tail -f /var/log/syslog
```

### 组合使用`head`和`tail`
有时你可能想查看文件的中间部分，可以组合使用`head`和`tail`命令。例如，查看文件的第11到第20行：

```plain
head -n 20 /path/to/logfile | tail -n 10
```

这条命令首先使用`head -n 20`查看前20行，然后通过管道传递给`tail -n 10`，从中提取出最后10行，即第11到第20行。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/fuxniugwryvossnx>