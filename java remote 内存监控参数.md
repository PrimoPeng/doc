---
title: java remote 内存监控参数
tags: java,jmx
---
### 开启远程内存监控参数
nohup java -Xmx8G -Xms4G -Xmn2G -XX:SurvivorRatio=2 \
-XX:TargetSurvivorRatio=90 -XX:-UseGCOverheadLimit \
-XX:MaxTenuringThreshold=15 \
-Dcom.sun.management.jmxremote=true \
-Djava.rmi.server.hostname=192.168.0.118 \
-Dcom.sun.management.jmxremote.port=9010 \
-Dcom.sun.management.jmxremote.local.only=false \
-Dcom.sun.management.jmxremote.authenticate=false \
-Dcom.sun.management.jmxremote.ssl=false \
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 \
-jar xiaodirobot-1.0-SNAPSHOT.jar > /dev/null &

### 开启远程调试 参数
-Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,address=9999,server=y,suspend=y

--开启远程gc minitor
nohup jstatd -J-Djava.security.policy=/tmp/tools.policy \
-J-Djava.rmi.server.hostname=192.168.1.118 -p 1090 &