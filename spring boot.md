---
title: spring boot 
tags: spring, boot
grammar_cjkRuby: true
---
### Application Property Files
SpringApplication loads properties from application.properties files in the following locations and adds them to the Spring Environment:

 1. A /config subdirectory of the current directory
 2. The current directory
 3. A classpath /config package
 4. The classpath root。

### spring framework log 配置
```
<!--Jakarta Commons Logging (JCL) API-->
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>jcl-over-slf4j</artifactId>
	<version>1.7.21</version>
</dependency>
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-log4j12</artifactId>
	<version>1.7.21</version>
</dependency>
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>
```
Solved. Just need you put log4j.properties file in your classpath. then you will get your result.


