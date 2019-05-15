---
title: ubuntu 安装 mysql 
tags: ubuntu, mysql
grammar_cjkRuby: true
---

### 下载MySQL APT Repository
```
 http://dev.mysql.com/downloads/repo/apt/
```

### Adding the MySQL APT Repository
```
sudo dpkg -i /PATH/version-specific-package-name.deb
```

### Update package information from the MySQL APT repository 
```
sudo apt-get update
```

### Installing MySQL with APT
```
shell> sudo apt-get install mysql-server
```

### Starting and Stopping the MySQL Server
```
shell> sudo service mysql status

shell> sudo service mysql stop

shell> sudo service mysql start
```