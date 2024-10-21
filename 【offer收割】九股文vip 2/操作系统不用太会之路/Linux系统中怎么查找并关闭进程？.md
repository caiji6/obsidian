# 👌Linux系统中怎么查找并关闭进程？

查看关联的进程命令：

```plain
ps -ef | grep 进程名
```

也可以根据某一端口来反查

```plain
lsof -i:端口号
netstat -nap | grep 端口号
```

```plain
kill -9 进程号
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/gu947xo8mp8bb61n>