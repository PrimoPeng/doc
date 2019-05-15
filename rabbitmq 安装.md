---
title: rabbitmq 安装
tags: rabbitmq
---

### rabbitmq 安装
1. 安装Erlang
```
$ wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
$ sudo dpkg -i erlang-solutions_1.0_all.deb

$ sudo apt-get update
$ sudo apt-get install erlang

$ sudo apt-get update
$ sudo apt-get install esl-erlang
```

2. 安装rabbitmq仓库

So, on Ubuntu 16.04 the above command becomes
```
echo "deb https://dl.bintray.com/rabbitmq/debian xenial main" | sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list
```

Next add our public key to your trusted key list using apt-key(8):
```
wget -O- https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc |
     sudo apt-key add -
```

3.安装rabbitmq

```
sudo apt-get update
sudo apt-get install rabbitmq-server
```

4.设置账户

```
rabbitmqctl add_user admin admin
rabbitmqctl set_user_tags admin administrator
rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
```

5.开通http服务

```
rabbitmq-plugins enable rabbitmq_management
```