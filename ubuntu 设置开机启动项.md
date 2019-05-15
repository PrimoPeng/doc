---
title: ubuntu 设置开机启动项 
tags: ubuntu,启动项
---

### 方法一
Ubuntu开机之后会自动执行/etc/rc.local文件中的脚本，所以我们可以直接在/etc/rc.local中添加启动脚本，但必须添加到语句exit 0前面才行。

### 方法二

 1. 将你的启动脚本复制到/etc/init.d目录下（以下假设你的脚本为test）
 2. 设置脚本文件的权限
```
$sudo chmod 775 /etc/init.d/test
```
 3. 执行如下命令脚本放到启动脚本中去
```
$cd /etc/init.d
$sudo update-rc.d test defaults 100 
```
需要注意的是数字95是脚本的启动顺序号，按照自己的需要相应的修改即可。在你有多个启动脚本，而它们之间又有先后启动的依赖关系时你就知道这个数字的具体作用了。
 4. 卸载启动脚本
```
$cd /etc/init.d
$sudo update-rc.d -f test remove 
```
 5. 为脚本设置LSB头
 ```
#! /bin/sh
### BEGIN INIT INFO
# Provides:          rc.local
# Required-Start:    $all
# Required-Stop:
# Default-Start:     2 3 4 5
# Default-Stop:
# Short-Description: Run /etc/rc.local if it exist
### END INIT INFO

```